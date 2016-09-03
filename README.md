# nginx-faq

* How does match uri in nginx

  check the repo [nginx-location-match-visible](https://github.com/detailyang/nginx-location-match-visible)

* Is blocked on nginx write log

  Yes, it's blocked. the ngx_http_log_handler are working on the nginx log phase. It use block IO to write log, But you can reduce IO Operation by using the `buffer` and `flush` according by the [docs](http://nginx.org/en/docs/http/ngx_http_log_module.html#access_log)
