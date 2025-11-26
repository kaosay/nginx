# nginx
high avaliable nginx configuration

## Add brotli module to nginx

git clone --recurse-submodules https://github.com/google/ngx_brotli.git

wget https://nginx.org/download/nginx-1.24.0.tar.gz

tar xvf nginx-1.24.0.tar.gz

sudo apt install build-essential libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev git cmake curl unzip libxml2-dev libxslt1-dev libgd-dev libgeoip-dev libperl-dev perl-base

./configure --with-cc-opt='-g -O2 -fno-omit-frame-pointer -mno-omit-leaf-frame-pointer -ffile-prefix-map=/build/nginx-WLuzPu/nginx-1.24.0=. -flto=auto -ffat-lto-objects -fstack-protector-strong -fstack-clash-protection -Wformat -Werror=format-security -fcf-protection -fdebug-prefix-map=/build/nginx-WLuzPu/nginx-1.24.0=/usr/src/nginx-1.24.0-2ubuntu7.5 -fPIC -Wdate-time -D_FORTIFY_SOURCE=3' --with-ld-opt='-Wl,-Bsymbolic-functions -flto=auto -ffat-lto-objects -Wl,-z,relro -Wl,-z,now -fPIC' --prefix=/usr/share/nginx --conf-path=/etc/nginx/nginx.conf --http-log-path=/var/log/nginx/access.log --error-log-path=stderr --lock-path=/var/lock/nginx.lock --pid-path=/run/nginx.pid --modules-path=/usr/lib/nginx/modules --http-client-body-temp-path=/var/lib/nginx/body --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-scgi-temp-path=/var/lib/nginx/scgi --http-uwsgi-temp-path=/var/lib/nginx/uwsgi --with-compat --with-debug --with-pcre-jit --with-http_ssl_module --with-http_stub_status_module --with-http_realip_module --with-http_auth_request_module --with-http_v2_module --with-http_dav_module --with-http_slice_module --with-threads --with-http_addition_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_secure_link_module --with-http_sub_module --with-mail_ssl_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-stream_realip_module --with-http_geoip_module=dynamic --with-http_image_filter_module=dynamic --with-http_perl_module=dynamic --with-http_xslt_module=dynamic --with-mail=dynamic --with-stream=dynamic --with-stream_geoip_module=dynamic --with-compat --add-dynamic-module=../ngx_brotli

make modules

sudo mkdir -p /usr/lib/nginx/modules
sudo cp objs/ngx_http_brotli_filter_module.so /usr/lib/nginx/modules/
sudo cp objs/ngx_http_brotli_static_module.so /usr/lib/nginx/modules/

# Edit nginx.conf
load_module /usr/lib/nginx/modules/ngx_http_brotli_filter_module.so;
load_module /usr/lib/nginx/modules/ngx_http_brotli_static_module.so;

http {
    brotli on;                    # 启用 Brotli
    brotli_comp_level 6;          # 压缩级别（1-11，默认 6，平衡速度/压缩率）
    brotli_static on;             # 启用预压缩文件支持（.br 文件）
    brotli_types
        text/plain
        text/css
        application/javascript
        application/x-javascript
        text/xml
        application/xml
        application/xml+rss
        text/javascript
        image/svg+xml;            # 指定压缩 MIME 类型

    gzip on;
    gzip_static on;
    gzip_types 同上;
}

