---
# vim: tw=82
title: How to roll your own CDN
layout: post
---

We have been serving media for over a year now, and for most of that time we have
been relying on a single dedicated server, located in Germany. We experimented with turnkey
solutions like CloudFlare and CloudFront for content delivery. While they worked, 
and the performance was decent, they ultimately turned out to be too expensive for 
our needs. 

Over the last couple of weeks we have deployed a couple of edge nodes, and added
them to a location-aware DNS service. The preliminary results are very promising,
so we wanted to share how we did it.

## Point-to-point encrypted links

We have a very strong stance on privacy, so unencrypted links between datacentres
were simply [not an option](https://mediacru.sh/ivXT2SM0UI9q). We chose to use
OpenVPN to secure these links. 

Assuming OpenVPN and easy-rsa are installed, we start setting up our public key
infrastructure by generating a certificate authority: 

    # ./build-ca
    Generating a 2048 bit RSA private key
    ..............++++++
    ...++++++
    writing new private key to 'ca.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [US]: UK 
    State or Province Name (full name) [CA]: UK
    Locality Name (eg, city) [Acme Acres]: MediaCrush
    Organization Name (eg, company) [Acme]: MediaCrush
    Organizational Unit Name (eg, section) []:
    Common Name (eg, your name or your server's hostname) [Acme-CA]: MediaCrush-CA
    Name [Acme-CA]: MediaCrush-CA
    Email Address [roadrunner@acmecorp.org]: admin@mediacru.sh

The next thing we need to do is generate a public-private key pair for the server. 

    # ./build-key-server server 
    Generating a 2048 bit RSA private key
    .....................++++++
    .......................................................++++++
    writing new private key to 'server.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [US]: UK
    State or Province Name (full name) [CA]: UK
    Locality Name (eg, city) [Acme Acres]: MediaCrush
    Organization Name (eg, company) [Acme]: MediaCrush
    Organizational Unit Name (eg, section) []:
    Common Name (eg, your name or your server's hostname) [server]:
    Name [server]:
    Email Address [admin@mediacru.sh]:

    Please enter the following 'extra' attributes
    to be sent with your certificate request
    A challenge password []:
    An optional company name []:
    Using configuration from /root/easy-rsa/openssl-1.0.0.cnf
    Check that the request matches the signature
    Signature ok
    The Subject's Distinguished Name is as follows
    countryName           :PRINTABLE:'UK'
    stateOrProvinceName   :PRINTABLE:'UK'
    localityName          :PRINTABLE:'MediaCrush'
    organizationName      :PRINTABLE:'MediaCrush'
    commonName            :PRINTABLE:'server'
    name                  :PRINTABLE:'server'
    emailAddress          :IA5STRING:'admin@mediacru.sh'
    Certificate is to be certified until Aug 15 19:11:59 2021 GMT (3650 days)
    Sign the certificate? [y/n]:y

    1 out of 1 certificate requests certified, commit? [y/n]y
    Write out database with 1 new entries
    Data Base Updated

We'll do the same thing to generate a key pair for the edge node (ommitted for
brevity). The only thing left to do to complete our PKI is generate the
Diffie-Hellman parameters. We do this by executing `build-dh`:

    # ./build-dh
    Generating DH parameters, 2048 bit long safe prime, generator 2
    This is going to take a long time
    ..+.............................................................................
    .
    .
    .
    ............+...............+...................................................
    ..................................................................++*++*
    (etc)

Next, we'll some files to `/etc/openvpn`:

    # cp ca.crt ca.key dh2048.pem server.crt server.key /etc/openvpn

Now we're ready to configure the OpenVPN server. It should be pretty close to the
stock configuration. Here's what we use:

    port 1194
    proto udp
    dev tun

    ca ca.crt
    cert server.crt
    key server.key
    dh dh2048.pem

    server 10.8.0.0 255.255.255.0

    ifconfig-pool-persist ipp.txt
    keepalive 10 120

    persist-key
    persist-tun

    status openvpn-status.log
    verb 3

Next thing you should do is start the OpenVPN server. Unfortunately every Linux
distribution likes to do these things differently, but in Arch (which is our
distro of choice), you would do `systemctl start openvpn@server`.

You will need to copy the edge's key pair and certificate authority to each node
you deploy. In our case, these files are ca.crt, cdn-us-1.crt, and cdn-us-1.key.
Our OpenVPN client config on the edges looks like this:

    client

    dev tun
    proto udp

    remote mediacru.sh 1194
    resolv-retry infinite
    nobind

    persist-key
    persist-tun

    ca ca.crt
    cert cdn-us-1.crt
    key cdn-us-1.key

    ns-cert-type server

    verb 3

Again, we start the OpenVPN client on the edge and verify connectivity to the
master server:

    jdiez@mediacrush-us-1:~$ ping 10.8.0.1
    PING 10.8.0.1 (10.8.0.1) 56(84) bytes of data.
    64 bytes from 10.8.0.1: icmp_req=1 ttl=64 time=170 ms
    64 bytes from 10.8.0.1: icmp_req=2 ttl=64 time=170 ms
    ^C
    --- 10.8.0.1 ping statistics ---
    2 packets transmitted, 2 received, 0% packet loss, time 1001ms
    rtt min/avg/max/mdev = 170.222/170.264/170.307/0.414 ms
    jdiez@mediacrush-us-1:~$ 

## Configuring nginx as a caching proxy

This is what we spent most time tweaking. Our first approach was to use Varnish to
handle the caching, but after much experimenting (and headaches) we settled on nginx, 
which turns out to be significantly better for our use case (which is caching big 
files and being able to return partial responses even if the object is not in cache).

The key was to [disable gzip
compression](https://github.com/MediaCrush/MediaCrush/commit/43e9bf7629e3c98e429b406298c922f8821c3ad2)
for static files (which wouldn't help, anyway). The problem with gzip is that we
don't know what the Content-Length is, and therefore we couldn't respond to a HTTP
206 request properly. This took a while to figure out.

Once that was out of the way, the rest was relatively easy. Here's our prospective
nginx edge config:

    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:ECDHE-RSA-RC4-SHA:ECDHE-ECDSA-RC4-SHA:AES128:AES256:RC4-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!3DES:!MD5:!PSK; ssl_session_timeout 5m;
    ssl_prefer_server_ciphers on;
    ssl_certificate /etc/ssl/mediacru.sh.crt;
    ssl_certificate_key /etc/nginx/mediacru.sh.key;
    ssl_session_cache shared:SSL:50m;
    ssl_dhparam /etc/nginx/dhparam.pem;
    ssl_protocols TLSv1.2 TLSv1.1 TLSv1;
    resolver 8.8.8.8;

    proxy_cache_path /var/nginx/cache keys_zone=static:50m
             loader_threshold=300 loader_files=700 inactive=24h
             max_size=10g;

    server {
        listen 443 ssl spdy default_server;
        server_name _;
        access_log /var/log/nginx/access.log main;

        location / {
            proxy_pass http://10.8.0.1/;
        }

        location ~ ^/([A-Za-z0-9_-]+\.(gif|mp4|ogv|png|mp3|svg|ogg|jpe|jpg|jpeg|webm|zip))$ {
            proxy_pass http://10.8.0.1/$1;
            proxy_cache_key "$request_uri";
            proxy_cache_valid 200 24h;
            proxy_cache_valid any 3s;
            proxy_cache_revalidate on;
            proxy_cache_min_uses 1;
            proxy_cache static;
            add_header X-Cache $upstream_cache_status;
        }
    }

The first few lines are our SSL configuration, with carefully chosen cipher suites
and SSL features. The first relevant directive is proxy_cache_path. Let's break it
down:

* `/var/nginx/cache/` is the folder where the cached objects will be stored on
disk.
* `keys_zone=static:50m` defines the caching zone. More complicated setups might
use different zones, but it's unnecessary in our case. The second parameter
specifies a shared memory of 50MB, used to store metadata about cached objects.
This is shared between nginx workers. The [nginx
documentation](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_path)
says that one megabyte of shared memory is enough for 8 thousand objects.
* `loader_threshold` and `loader_files` are used to tweak the performance of the
cache, but they shouldn't matter too much. 
* `inactive=24h` tells nginx to wait 24h before evicting objects from the cache if
they haven't been hit.
* `max_size=10g` limits the total storage space used up by the cache. This should
be as large as your hardware allows.

This is what the rest of the cache directives do:

* `proxy_cache_key "$request_uri"` indexes cached objects by request URL (which
will be the file hash and extension, in our case).
* `proxy_cache_valid` specifies the time period after which objects are considered
stale. In our case, we will keep successful requests around for a day and any
other request for three seconds.
* `proxy_cache_revalidate on` will reset the expiration date if the file hasn't
changed since the last request.
* `proxy_cache_min_uses 1` tells nginx to cache every file the first time it
sees it. However if we set it 5, for example, it wouldn't cache it until it had
been hit 5 times.
* `proxy_cache static` chooses which cache zone to use (`static` in our case).

## Choosing the closest server to handle requests

A key piece of this architecture is being able to route users' request to the
server that is closest to them geographically. This is done using a technique
called [DNS Anycast](http://en.wikipedia.org/wiki/Anycast). In some cases, Anycast
is used for reliability, where every DNS server in each availability zone has the
same DNS records. However we have different records in each availability zone; for
example, in our US DNS servers we have `cdn.mediacru.sh` as a CNAME that points to
`cdn-us-1.mediacru.sh`. Our DNS server in Singapore would reply with a CNAME to
`cdn-asia-1.mediacru.sh`, and so on.

-- insert dns screenie here --

## Results


