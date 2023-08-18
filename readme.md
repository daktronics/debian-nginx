# Debian Nginx
Our own in-house flavor of Nginx for Debian:
- Forked from: https://salsa.debian.org/nginx-team/nginx

Documentation is available at http://nginx.org

## Building
Building this instance of nginx is very similar to the normal nginx version. Really, the only difference is in the packages used.
- In the most recent version, we use:
    - PCRE2, version 10.42 ([link](https://github.com/PCRE2Project/pcre2/releases/tag/pcre2-10.42)) ([github](https://github.com/PCRE2Project/pcre2))
    - OpenSSL, version 1.1.1u ([tar.gz](https://www.openssl.org/source/openssl-1.1.1u.tar.gz)) ([github](https://github.com/openssl/openssl))
    - zlib, version 1.2.13 ([tar.gz](https://zlib.net/zlib-1.2.13.tar.gz)) ([main page](https://zlib.net/)) ([github]{https://github.com/madler/zlib})
- The most recent configuration string used is this:
```
--with-cc-opt='-g -O2 -fdebug-prefix-map=/build/nginx-1.18.0=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' 
--with-ld-opt='-Wl,-z,relro -Wl,-z,now -fPIC' 
--prefix=/usr/share/nginx 
--conf-path=/etc/nginx/nginx.conf 
--http-log-path=/var/log/nginx/access.log 
--error-log-path=/var/log/nginx/error.log 
--lock-path=/var/lock/nginx.lock 
--pid-path=/run/nginx.pid 
--modules-path=/usr/lib/nginx/modules 
--http-client-body-temp-path=/var/lib/nginx/body 
--http-fastcgi-temp-path=/var/lib/nginx/fastcgi 
--http-proxy-temp-path=/var/lib/nginx/proxy 
--http-scgi-temp-path=/var/lib/nginx/scgi 
--http-uwsgi-temp-path=/var/lib/nginx/uwsgi 
--with-compat 
--with-debug 
--with-pcre-jit 
--with-http_ssl_module 
--with-http_stub_status_module 
--with-http_realip_module 
--with-http_auth_request_module 
--with-http_v2_module 
--with-http_dav_module 
--with-http_slice_module 
--with-threads 
--with-http_addition_module 
--with-http_gunzip_module 
--with-http_gzip_static_module 
--with-http_image_filter_module=dynamic 
--with-http_random_index_module 
--with-http_secure_link_module 
--with-http_sub_module 
--with-stream=dynamic 
--with-stream_ssl_module 
--with-stream_ssl_preread_module 
--add-module=src/nginx-http-auth-digest 
--with-pcre="libs/pcre2-10.42" 
--with-openssl="libs/openssl-1.1.1u"
--with-zlib="libs/zlib-1.2.13"
```
- Things to remember:
    - If there are build issues, ***make sure*** that all the files are using ``LF`` and not ``CRLF`` for line endings (when building in Linux, that is).
    - For the ``--with-http_image_filter_module=dynamic`` option, the GD library will need to be installed (if it isn't already). The quickest way would be ``sudo apt install libgd-dev`` in the command-line (on Linux, that is).
    - If g++ is missing, run ``sudo apt-get install g++`` in the command-line (on Linux, that is).