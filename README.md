# nginx-openresty-dynamic-ssl
nginx openresty dynamic ssl lua script + cache

There is an auto letsencrypt nginx lua module out there. But good luck using it and using it safely in a load-balanced environment and without letsencrypt.

With this script, cert generation is a different process(not included) and this is only using the pre-generated certs/keys.

### how to use it

Default configuration is in the lua script. And dir/file structure as an example:
/etc/letsencrypt/$product_id/$sni_name.crt
/etc/letsencrypt/$product_id/$sni_name.key

Ofc you can change these and do what you want with the script and use it as a baseline.

```
http {
...
    # global shared variables for the cert cache
    lua_shared_dict certs 16m;
    lua_shared_dict certs_content 32m;
...
}
...

# example server block
server {
        listen   1.2.3.4:443 ssl http2;

        ssl_certificate /etc/nginx/ssl/dummy.crt;
        ssl_certificate_key /etc/nginx/ddl/dummy.key;
        ssl_certificate_by_lua_file /usr/local/openresty/nginx/conf/conf.d/lua-ssl-product-1.lua;
        
        ...
        ...
}
```
