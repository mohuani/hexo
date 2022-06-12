---
title: "nginx关键字"
date: 2018-06-22 21:22:30
draft: true
tags: [NGINX]
categories:
- [NGINX]
---

对关键字的解释会慢慢加上

master进程：
worker进程：
pass_proxy

------

core Nginx.c：
daemon
master_process
timer_resolution
pid
lock_file
worker_processes
debug_points
user
worker_priority
worker_cpu_affinity
worker_rlimit_nofile
worker_rlimit_core
worker_rlimit_sigpending
working_directory
worker_threads
thread_stack_size


core Ngx_conf_file.c:
include


event Ngx_devpoll_modules.c:
devpoll_changes
devpoll_events


event Ngx_epoll_modules.c:
epoll_events


event Ngx_event.c:
events
worker_connections
connections
use
multi_accept
accept_mutex
accept_mutex_delay
debug_connection


core Ngx_eventport_modules.c:
eventport_events


core Ngx_event_openssl.c:
ssl_engine


http Ngx_http.c:
http


http modules Ngx_http_access_modules.c:
allow
deny


http modules Ngx_http_addition_filter_modules.c:
add_before_body
add_after_body


http modules Ngx_http_auth_basic_modules.c:
auth_basic
auth_basic_user_file


http modules Ngx_http_autoindex_module.c:
autoindex
autoindex_localtime
autoindex_exact_size


http modules Ngx_http_browser_module.c:
modern_browser
ancient_browser
modern_browser_value
ancient_browser_value


http modules Ngx_http_charset_filter_module.c:
charset
source_charset
override_charset
charset_map


http Ngx_http_copy_filter_module.c:
output_buffers


http Ngx_http_core_module.c:
variables_hash_max_size
variables_hash_bucket_size
server_names_hash_max_size
server_names_hash_bucket_size
server
connection_pool_size
request_pool_size
client_header_timeout
client_header_buffer_size
large_client_header_buffers
optimize_server_names
optimize_host_names
ignore_invalid_headers
location
listen
server_name
types_hash_max_size
types_hash_bucket_size
types
default_type
root
alias
limit_except
client_max_body_size
client_body_buffer_size
client_body_timeout
client_body_temp_path
client_body_in_file_only
sendfile
tcp_nopush
tcp_nodelay
send_timeout
send_lowat
postpone_output
limit_rate
keepalive_timeout
satisfy_any
internal
lingering_time
lingering_timeout
reset_timedout_connection
port_in_redirect
msie_padding
msie_refresh
log_not_found
recursive_error_pages
error_page
post_action
error_log
open_file_cache


http modules ngx_http_dav_module.c:
dav_methods
create_full_put_path
dav_access


http modules ngx_http_empty_gif_module.c:
empty_gif


http modules ngx_http_fastcgi_module.c:
fastcgi_pass
fastcgi_index
fastcgi_ignore_client_abort
fastcgi_connect_timeout
fastcgi_send_timeout
fastcgi_send_lowat
fastcgi_buffer_size
fastcgi_header_buffer_size
fastcgi_pass_request_headers
fastcgi_pass_request_body
fastcgi_intercept_errors
fastcgi_redirect_errors
fastcgi_read_timeout
fastcgi_buffers
fastcgi_busy_buffers_size
fastcgi_temp_path
fastcgi_max_temp_file_size
fastcgi_temp_file_write_size
fastcgi_next_upstream
fastcgi_upstream_max_fails
fastcgi_upstream_fail_timeout
fastcgi_param
fastcgi_pass_header
fastcgi_hide_header


http modules ngx_http_flv_modules.c:
flv


http modules ngx_http_geo_modules.c:
geo


http modules ngx_http_gzip_filter_modules.c:
gzip
gzip_buffers
gzip_types
gzip_comp_level
gzip_window
gzip_hash
gzip_no_buffer
gzip_http_version
gzip_proxied
gzip_min_length


http modules ngx_http_headers_filter_modules.c:
expires
add_header


http modules ngx_http_index_modules.c:
index
index_cache


http modules ngx_http_log_modules.c:
log_format
access_log


http modules ngx_http_map_modules.c:
map
map_hash_max_size
map_hash_bucket_size


http modules ngx_http_memcached_modules.c:
memcached_pass
memcached_connect_timeout
memcached_send_timeout
memcached_buffer_size
memcached_read_timeout
memcached_next_upstream
memcached_upstream_max_fails
memcached_upstream_fail_timeout


mysql ngx_http_mysql_test.c
mysql_test


http modules ngx_http_perl_modules.c:
perl_modules
perl_require
perl
perl_set


http modules ngx_http_proxy_modules.c:
proxy_pass
proxy_redirect
proxy_buffering
proxy_ignore_client_abort
proxy_connect_timeout
proxy_send_timeout
proxy_send_lowat
proxy_intercept_errors
proxy_redirect_errors
proxy_set_header
proxy_set_body
proxy_method
proxy_pass_request_headers
proxy_pass_request_body
proxy_buffer_size
proxy_header_buffer_size
proxy_read_timeout
proxy_buffers
proxy_busy_buffers_size
proxy_temp_path
proxy_max_temp_file_size
proxy_temp_file_write_size
proxy_next_upstream
proxy_upstream_max_fails
proxy_upstream_fail_timeout
proxy_pass_header
proxy_hide_header


http modules ngx_http_realip_module.c:
set_real_ip_from
real_ip_header


http modules ngx_http_referer_module.c:
valid_referers


http modules ngx_http_rewrite_module.c:
rewrite
return
break
if
set
rewrite_log
uninitialized_variable_warn


http modules ngx_http_ssi_filter_module.c:
ssi
ssi_silent_errors
ssi_ignore_recycled_buffers
ssi_min_file_chunk
ssi_value_length
ssi_types


http modules Ngx_http_ssl_module.c:
ssl
ssl_certificate
ssl_certificate_key
ssl_protocols
ssl_ciphers
ssl_verify_client
ssl_verify_depth
ssl_client_certificate
ssl_prefer_server_ciphers
ssl_session_timeout


http modules Ngx_http_static_module.c:
redirect_cache


http modules Ngx_http_status_module.c:
status


http modules Ngx_http_stub_status_module.c:
stub_status


http Ngx_http_upstream.c:
upstream
server


http modules Ngx_http_upstream_ip_hash_module.c：
ip_hash


http modules Ngx_http_userid_filter_module.c：
userid
userid_service
userid_name
userid_domain
userid_path
userid_expires
userid_p3p
userid_mark


imap Ngx_imap.c：
imap


imap Ngx_imap_auth_http_module.c:
auth_http
auth_http_timeout
auth_http_header


imap Ngx_imap_core_module.c:
server
listen
protocol
imap_client_buffer
so_keepalive
timeout
pop3_capabilities
imap_capabilities
server_name
auth


imap Ngx_imap_proxy_module.c:
proxy
proxy_buffer
proxy_timeout
proxy_pass_error_message


imap Ngx_imap_ssl_module.c:
ssl
starttls
ssl_certificate
ssl_certificate_key
ssl_protocols
ssl_ciphers
ssl_prefer_server_ciphers
ssl_session_timeout


event modules Ngx_iocp_module.c:
iocp_threads
post_acceptex
acceptex_read


event modules Ngx_kqueue_module.c:
kqueue_changes
kqueue_events


core Ngx_log.c:
error_log


event modules Ngx_rtsig_module.c:
rtsig_signo
rtsig_overflow_events
rtsig_overflow_test
rtsig_overflow_threshold