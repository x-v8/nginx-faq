# nginx-faq

* How does match uri in nginx ?

  check the repo [nginx-location-match-visible](https://github.com/detailyang/nginx-location-match-visible)

* Is blocked on nginx write log ?

  Yes, it's blocked. the ngx_http_log_handler are working on the nginx log phase. It use block IO to write log, But you can reduce IO Operation by using the `buffer` and `flush` according by the [docs](http://nginx.org/en/docs/http/ngx_http_log_module.html#access_log)

* proxy_set_header merge question
  

  someone are using the config as the following:
  
  ````bash
  server {
    listen 80;
    server_name example.com;

    proxy_set_header foo 1;
    location / {
      proxy_set_header bar 2;
      proxy_set_header host $http_host;
      proxy_pass http://xx;
    }
  }
  ````
  
  we expect the header to upstream shoule be like this
  
  ````bash
  foo 1
  bar: 2
  host: example.com
  ````
  
  In fact it is
  
  ````bash
  bar: 2
  host: example.com
  ````
  
  what the hell it is? It's because of the ngx_http_proxy_module's proxy_set_header do not merge the conf by key like header[k] v, it's just reference the new header the code like this
  
  ````bash
  if (conf->headers == NULL) conf->headers = prev->headers.
  ````
    
  I suggest you can use the [header-more-nginx-module](https://github.com/openresty/headers-more-nginx-module) to replace proxy_set_header:)
