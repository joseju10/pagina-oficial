# Compilación de un programa en C utilizando un Makefile.

## NGINX

El programa que he elegido para realizar la compilación es nginx, para compilarlo en nuestro equipo seguiremos los siguientes pasos:

### Pasos iniciales

Seguiremos los siguientes pasos para compilar nginx en nuestro equipo, para ello hacemos lo siguiente:

- Comprobamos la versión del sistema:

```shell
joseju@joseju:~$ lsb_release -ds
Debian GNU/Linux 11 (bullseye)
```

- Instalamos paquetes necesarios si no lo están:

```shell
joseju@joseju:~$ sudo apt install -y build-essential git tree software-properties-common dirmngr apt-transport-https ufw curl wget
```

- Actualizamos paquetería:

```shell
joseju@joseju:~$ sudo apt update -y && sudo apt upgrade -y
Obj:1 http://deb.debian.org/debian bullseye InRelease
Obj:2 http://deb.debian.org/debian bullseye-updates InRelease     
Obj:3 http://security.debian.org/debian-security bullseye-security InRelease
Obj:4 http://packages.microsoft.com/repos/code stable InRelease   
Leyendo lista de paquetes... Hecho      
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Todos los paquetes están actualizados.
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Calculando la actualización... Hecho
0 actualizados, 0 nuevos se instalarán, 0 para eliminar y 0 no actualizados.
```

### Compilación

En este punto realizaremos la compilación del programa nginx, para ello haremos lo siguiente:

- Descargamos el programa comprimido de nginx, en concreto la versión 1.15.8:

```shell
joseju@joseju:~/nginx$ wget https://nginx.org/download/nginx-1.15.8.tar.gz && tar zxvf nginx-1.15.8.tar.gz
--2022-11-29 21:26:25--  https://nginx.org/download/nginx-1.15.8.tar.gz
Resolviendo nginx.org (nginx.org)... 3.125.197.172, 52.58.199.22, 2a05:d014:edb:5704::6, ...
Conectando con nginx.org (nginx.org)[3.125.197.172]:443... conectado.
Petición HTTP enviada, esperando respuesta... 200 OK
Longitud: 1027862 (1004K) [application/octet-stream]
Grabando a: «nginx-1.15.8.tar.gz»

nginx-1.15.8.tar.gz 100%[===================>]   1004K  1,19MB/s    en 0,8s    

2022-11-29 21:26:26 (1,19 MB/s) - «nginx-1.15.8.tar.gz» guardado [1027862/1027862]

nginx-1.15.8/
nginx-1.15.8/auto/
nginx-1.15.8/conf/
nginx-1.15.8/contrib/
nginx-1.15.8/src/
nginx-1.15.8/configure
nginx-1.15.8/LICENSE
nginx-1.15.8/README
nginx-1.15.8/html/
nginx-1.15.8/man/
nginx-1.15.8/CHANGES.ru
nginx-1.15.8/CHANGES
nginx-1.15.8/man/nginx.8
nginx-1.15.8/html/50x.html
nginx-1.15.8/html/index.html
nginx-1.15.8/src/core/
nginx-1.15.8/src/event/
nginx-1.15.8/src/http/
nginx-1.15.8/src/mail/
nginx-1.15.8/src/misc/
nginx-1.15.8/src/os/
nginx-1.15.8/src/stream/
nginx-1.15.8/src/stream/ngx_stream.c
nginx-1.15.8/src/stream/ngx_stream.h
nginx-1.15.8/src/stream/ngx_stream_access_module.c
nginx-1.15.8/src/stream/ngx_stream_core_module.c
nginx-1.15.8/src/stream/ngx_stream_geo_module.c
nginx-1.15.8/src/stream/ngx_stream_geoip_module.c
nginx-1.15.8/src/stream/ngx_stream_handler.c
nginx-1.15.8/src/stream/ngx_stream_limit_conn_module.c
nginx-1.15.8/src/stream/ngx_stream_log_module.c
nginx-1.15.8/src/stream/ngx_stream_map_module.c
nginx-1.15.8/src/stream/ngx_stream_proxy_module.c
nginx-1.15.8/src/stream/ngx_stream_realip_module.c
nginx-1.15.8/src/stream/ngx_stream_return_module.c
nginx-1.15.8/src/stream/ngx_stream_script.c
nginx-1.15.8/src/stream/ngx_stream_script.h
nginx-1.15.8/src/stream/ngx_stream_split_clients_module.c
nginx-1.15.8/src/stream/ngx_stream_ssl_module.c
nginx-1.15.8/src/stream/ngx_stream_ssl_module.h
nginx-1.15.8/src/stream/ngx_stream_ssl_preread_module.c
nginx-1.15.8/src/stream/ngx_stream_upstream.c
nginx-1.15.8/src/stream/ngx_stream_upstream.h
nginx-1.15.8/src/stream/ngx_stream_upstream_hash_module.c
nginx-1.15.8/src/stream/ngx_stream_upstream_least_conn_module.c
nginx-1.15.8/src/stream/ngx_stream_upstream_random_module.c
nginx-1.15.8/src/stream/ngx_stream_upstream_round_robin.c
nginx-1.15.8/src/stream/ngx_stream_upstream_round_robin.h
nginx-1.15.8/src/stream/ngx_stream_upstream_zone_module.c
nginx-1.15.8/src/stream/ngx_stream_variables.c
nginx-1.15.8/src/stream/ngx_stream_variables.h
nginx-1.15.8/src/stream/ngx_stream_write_filter_module.c
nginx-1.15.8/src/os/unix/
nginx-1.15.8/src/os/unix/ngx_alloc.c
nginx-1.15.8/src/os/unix/ngx_alloc.h
nginx-1.15.8/src/os/unix/ngx_atomic.h
nginx-1.15.8/src/os/unix/ngx_channel.c
nginx-1.15.8/src/os/unix/ngx_channel.h
nginx-1.15.8/src/os/unix/ngx_daemon.c
nginx-1.15.8/src/os/unix/ngx_darwin.h
nginx-1.15.8/src/os/unix/ngx_darwin_config.h
nginx-1.15.8/src/os/unix/ngx_darwin_init.c
nginx-1.15.8/src/os/unix/ngx_darwin_sendfile_chain.c
nginx-1.15.8/src/os/unix/ngx_dlopen.c
nginx-1.15.8/src/os/unix/ngx_dlopen.h
nginx-1.15.8/src/os/unix/ngx_errno.c
nginx-1.15.8/src/os/unix/ngx_errno.h
nginx-1.15.8/src/os/unix/ngx_file_aio_read.c
nginx-1.15.8/src/os/unix/ngx_files.c
nginx-1.15.8/src/os/unix/ngx_files.h
nginx-1.15.8/src/os/unix/ngx_freebsd.h
nginx-1.15.8/src/os/unix/ngx_freebsd_config.h
nginx-1.15.8/src/os/unix/ngx_linux.h
nginx-1.15.8/src/os/unix/ngx_freebsd_init.c
nginx-1.15.8/src/os/unix/ngx_freebsd_sendfile_chain.c
nginx-1.15.8/src/os/unix/ngx_gcc_atomic_amd64.h
nginx-1.15.8/src/os/unix/ngx_gcc_atomic_ppc.h
nginx-1.15.8/src/os/unix/ngx_gcc_atomic_sparc64.h
nginx-1.15.8/src/os/unix/ngx_gcc_atomic_x86.h
nginx-1.15.8/src/os/unix/ngx_linux_aio_read.c
nginx-1.15.8/src/os/unix/ngx_linux_config.h
nginx-1.15.8/src/os/unix/ngx_linux_init.c
nginx-1.15.8/src/os/unix/ngx_linux_sendfile_chain.c
nginx-1.15.8/src/os/unix/ngx_os.h
nginx-1.15.8/src/os/unix/ngx_posix_config.h
nginx-1.15.8/src/os/unix/ngx_posix_init.c
nginx-1.15.8/src/os/unix/ngx_process.c
nginx-1.15.8/src/os/unix/ngx_process.h
nginx-1.15.8/src/os/unix/ngx_process_cycle.c
nginx-1.15.8/src/os/unix/ngx_process_cycle.h
nginx-1.15.8/src/os/unix/ngx_readv_chain.c
nginx-1.15.8/src/os/unix/ngx_recv.c
nginx-1.15.8/src/os/unix/ngx_send.c
nginx-1.15.8/src/os/unix/ngx_setaffinity.c
nginx-1.15.8/src/os/unix/ngx_setaffinity.h
nginx-1.15.8/src/os/unix/ngx_setproctitle.c
nginx-1.15.8/src/os/unix/ngx_setproctitle.h
nginx-1.15.8/src/os/unix/ngx_shmem.c
nginx-1.15.8/src/os/unix/ngx_shmem.h
nginx-1.15.8/src/os/unix/ngx_socket.c
nginx-1.15.8/src/os/unix/ngx_socket.h
nginx-1.15.8/src/os/unix/ngx_solaris.h
nginx-1.15.8/src/os/unix/ngx_solaris_config.h
nginx-1.15.8/src/os/unix/ngx_solaris_init.c
nginx-1.15.8/src/os/unix/ngx_solaris_sendfilev_chain.c
nginx-1.15.8/src/os/unix/ngx_sunpro_amd64.il
nginx-1.15.8/src/os/unix/ngx_sunpro_atomic_sparc64.h
nginx-1.15.8/src/os/unix/ngx_sunpro_sparc64.il
nginx-1.15.8/src/os/unix/ngx_thread.h
nginx-1.15.8/src/os/unix/ngx_sunpro_x86.il
nginx-1.15.8/src/os/unix/ngx_thread_cond.c
nginx-1.15.8/src/os/unix/ngx_thread_id.c
nginx-1.15.8/src/os/unix/ngx_thread_mutex.c
nginx-1.15.8/src/os/unix/ngx_time.c
nginx-1.15.8/src/os/unix/ngx_time.h
nginx-1.15.8/src/os/unix/ngx_udp_recv.c
nginx-1.15.8/src/os/unix/ngx_udp_send.c
nginx-1.15.8/src/os/unix/ngx_udp_sendmsg_chain.c
nginx-1.15.8/src/os/unix/ngx_user.c
nginx-1.15.8/src/os/unix/ngx_user.h
nginx-1.15.8/src/os/unix/ngx_writev_chain.c
nginx-1.15.8/src/misc/ngx_cpp_test_module.cpp
nginx-1.15.8/src/misc/ngx_google_perftools_module.c
nginx-1.15.8/src/mail/ngx_mail.c
nginx-1.15.8/src/mail/ngx_mail.h
nginx-1.15.8/src/mail/ngx_mail_auth_http_module.c
nginx-1.15.8/src/mail/ngx_mail_core_module.c
nginx-1.15.8/src/mail/ngx_mail_handler.c
nginx-1.15.8/src/mail/ngx_mail_imap_handler.c
nginx-1.15.8/src/mail/ngx_mail_imap_module.c
nginx-1.15.8/src/mail/ngx_mail_imap_module.h
nginx-1.15.8/src/mail/ngx_mail_parse.c
nginx-1.15.8/src/mail/ngx_mail_pop3_handler.c
nginx-1.15.8/src/mail/ngx_mail_pop3_module.c
nginx-1.15.8/src/mail/ngx_mail_pop3_module.h
nginx-1.15.8/src/mail/ngx_mail_proxy_module.c
nginx-1.15.8/src/mail/ngx_mail_smtp_handler.c
nginx-1.15.8/src/mail/ngx_mail_smtp_module.c
nginx-1.15.8/src/mail/ngx_mail_smtp_module.h
nginx-1.15.8/src/mail/ngx_mail_ssl_module.c
nginx-1.15.8/src/mail/ngx_mail_ssl_module.h
nginx-1.15.8/src/http/modules/
nginx-1.15.8/src/http/ngx_http.c
nginx-1.15.8/src/http/ngx_http.h
nginx-1.15.8/src/http/ngx_http_cache.h
nginx-1.15.8/src/http/ngx_http_config.h
nginx-1.15.8/src/http/ngx_http_copy_filter_module.c
nginx-1.15.8/src/http/ngx_http_core_module.c
nginx-1.15.8/src/http/ngx_http_core_module.h
nginx-1.15.8/src/http/ngx_http_file_cache.c
nginx-1.15.8/src/http/ngx_http_header_filter_module.c
nginx-1.15.8/src/http/ngx_http_parse.c
nginx-1.15.8/src/http/ngx_http_postpone_filter_module.c
nginx-1.15.8/src/http/ngx_http_request.c
nginx-1.15.8/src/http/ngx_http_request.h
nginx-1.15.8/src/http/ngx_http_request_body.c
nginx-1.15.8/src/http/ngx_http_script.c
nginx-1.15.8/src/http/v2/
nginx-1.15.8/src/http/ngx_http_script.h
nginx-1.15.8/src/http/ngx_http_special_response.c
nginx-1.15.8/src/http/ngx_http_upstream.c
nginx-1.15.8/src/http/ngx_http_upstream.h
nginx-1.15.8/src/http/ngx_http_upstream_round_robin.c
nginx-1.15.8/src/http/ngx_http_upstream_round_robin.h
nginx-1.15.8/src/http/ngx_http_variables.c
nginx-1.15.8/src/http/ngx_http_variables.h
nginx-1.15.8/src/http/ngx_http_write_filter_module.c
nginx-1.15.8/src/http/v2/ngx_http_v2.c
nginx-1.15.8/src/http/v2/ngx_http_v2.h
nginx-1.15.8/src/http/v2/ngx_http_v2_encode.c
nginx-1.15.8/src/http/v2/ngx_http_v2_filter_module.c
nginx-1.15.8/src/http/v2/ngx_http_v2_huff_decode.c
nginx-1.15.8/src/http/v2/ngx_http_v2_huff_encode.c
nginx-1.15.8/src/http/v2/ngx_http_v2_module.c
nginx-1.15.8/src/http/v2/ngx_http_v2_module.h
nginx-1.15.8/src/http/v2/ngx_http_v2_table.c
nginx-1.15.8/src/http/modules/ngx_http_access_module.c
nginx-1.15.8/src/http/modules/ngx_http_addition_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_auth_basic_module.c
nginx-1.15.8/src/http/modules/ngx_http_auth_request_module.c
nginx-1.15.8/src/http/modules/ngx_http_autoindex_module.c
nginx-1.15.8/src/http/modules/ngx_http_browser_module.c
nginx-1.15.8/src/http/modules/ngx_http_charset_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_chunked_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_dav_module.c
nginx-1.15.8/src/http/modules/ngx_http_degradation_module.c
nginx-1.15.8/src/http/modules/ngx_http_empty_gif_module.c
nginx-1.15.8/src/http/modules/ngx_http_fastcgi_module.c
nginx-1.15.8/src/http/modules/perl/
nginx-1.15.8/src/http/modules/ngx_http_flv_module.c
nginx-1.15.8/src/http/modules/ngx_http_geo_module.c
nginx-1.15.8/src/http/modules/ngx_http_geoip_module.c
nginx-1.15.8/src/http/modules/ngx_http_grpc_module.c
nginx-1.15.8/src/http/modules/ngx_http_gunzip_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_gzip_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_gzip_static_module.c
nginx-1.15.8/src/http/modules/ngx_http_headers_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_image_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_index_module.c
nginx-1.15.8/src/http/modules/ngx_http_limit_conn_module.c
nginx-1.15.8/src/http/modules/ngx_http_limit_req_module.c
nginx-1.15.8/src/http/modules/ngx_http_log_module.c
nginx-1.15.8/src/http/modules/ngx_http_map_module.c
nginx-1.15.8/src/http/modules/ngx_http_memcached_module.c
nginx-1.15.8/src/http/modules/ngx_http_mirror_module.c
nginx-1.15.8/src/http/modules/ngx_http_mp4_module.c
nginx-1.15.8/src/http/modules/ngx_http_not_modified_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_proxy_module.c
nginx-1.15.8/src/http/modules/ngx_http_random_index_module.c
nginx-1.15.8/src/http/modules/ngx_http_range_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_realip_module.c
nginx-1.15.8/src/http/modules/ngx_http_referer_module.c
nginx-1.15.8/src/http/modules/ngx_http_rewrite_module.c
nginx-1.15.8/src/http/modules/ngx_http_scgi_module.c
nginx-1.15.8/src/http/modules/ngx_http_secure_link_module.c
nginx-1.15.8/src/http/modules/ngx_http_slice_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_split_clients_module.c
nginx-1.15.8/src/http/modules/ngx_http_ssi_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_ssi_filter_module.h
nginx-1.15.8/src/http/modules/ngx_http_ssl_module.c
nginx-1.15.8/src/http/modules/ngx_http_ssl_module.h
nginx-1.15.8/src/http/modules/ngx_http_static_module.c
nginx-1.15.8/src/http/modules/ngx_http_stub_status_module.c
nginx-1.15.8/src/http/modules/ngx_http_sub_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_try_files_module.c
nginx-1.15.8/src/http/modules/ngx_http_upstream_hash_module.c
nginx-1.15.8/src/http/modules/ngx_http_upstream_ip_hash_module.c
nginx-1.15.8/src/http/modules/ngx_http_upstream_keepalive_module.c
nginx-1.15.8/src/http/modules/ngx_http_upstream_random_module.c
nginx-1.15.8/src/http/modules/ngx_http_upstream_least_conn_module.c
nginx-1.15.8/src/http/modules/ngx_http_upstream_zone_module.c
nginx-1.15.8/src/http/modules/ngx_http_userid_filter_module.c
nginx-1.15.8/src/http/modules/ngx_http_uwsgi_module.c
nginx-1.15.8/src/http/modules/ngx_http_xslt_filter_module.c
nginx-1.15.8/src/http/modules/perl/Makefile.PL
nginx-1.15.8/src/http/modules/perl/nginx.pm
nginx-1.15.8/src/http/modules/perl/nginx.xs
nginx-1.15.8/src/http/modules/perl/ngx_http_perl_module.c
nginx-1.15.8/src/http/modules/perl/ngx_http_perl_module.h
nginx-1.15.8/src/http/modules/perl/typemap
nginx-1.15.8/src/event/modules/
nginx-1.15.8/src/event/ngx_event.c
nginx-1.15.8/src/event/ngx_event.h
nginx-1.15.8/src/event/ngx_event_accept.c
nginx-1.15.8/src/event/ngx_event_connect.c
nginx-1.15.8/src/event/ngx_event_connect.h
nginx-1.15.8/src/event/ngx_event_openssl.c
nginx-1.15.8/src/event/ngx_event_openssl.h
nginx-1.15.8/src/event/ngx_event_openssl_stapling.c
nginx-1.15.8/src/event/ngx_event_pipe.c
nginx-1.15.8/src/event/ngx_event_pipe.h
nginx-1.15.8/src/event/ngx_event_posted.c
nginx-1.15.8/src/event/ngx_event_posted.h
nginx-1.15.8/src/event/ngx_event_timer.c
nginx-1.15.8/src/event/ngx_event_timer.h
nginx-1.15.8/src/event/ngx_event_udp.c
nginx-1.15.8/src/event/modules/ngx_devpoll_module.c
nginx-1.15.8/src/event/modules/ngx_epoll_module.c
nginx-1.15.8/src/event/modules/ngx_eventport_module.c
nginx-1.15.8/src/event/modules/ngx_kqueue_module.c
nginx-1.15.8/src/event/modules/ngx_poll_module.c
nginx-1.15.8/src/event/modules/ngx_select_module.c
nginx-1.15.8/src/event/modules/ngx_win32_select_module.c
nginx-1.15.8/src/core/nginx.c
nginx-1.15.8/src/core/nginx.h
nginx-1.15.8/src/core/ngx_array.c
nginx-1.15.8/src/core/ngx_array.h
nginx-1.15.8/src/core/ngx_buf.c
nginx-1.15.8/src/core/ngx_buf.h
nginx-1.15.8/src/core/ngx_conf_file.c
nginx-1.15.8/src/core/ngx_conf_file.h
nginx-1.15.8/src/core/ngx_config.h
nginx-1.15.8/src/core/ngx_connection.c
nginx-1.15.8/src/core/ngx_connection.h
nginx-1.15.8/src/core/ngx_core.h
nginx-1.15.8/src/core/ngx_cpuinfo.c
nginx-1.15.8/src/core/ngx_crc.h
nginx-1.15.8/src/core/ngx_crc32.c
nginx-1.15.8/src/core/ngx_crc32.h
nginx-1.15.8/src/core/ngx_crypt.c
nginx-1.15.8/src/core/ngx_crypt.h
nginx-1.15.8/src/core/ngx_cycle.c
nginx-1.15.8/src/core/ngx_cycle.h
nginx-1.15.8/src/core/ngx_file.c
nginx-1.15.8/src/core/ngx_file.h
nginx-1.15.8/src/core/ngx_hash.c
nginx-1.15.8/src/core/ngx_hash.h
nginx-1.15.8/src/core/ngx_inet.c
nginx-1.15.8/src/core/ngx_inet.h
nginx-1.15.8/src/core/ngx_list.c
nginx-1.15.8/src/core/ngx_list.h
nginx-1.15.8/src/core/ngx_log.c
nginx-1.15.8/src/core/ngx_log.h
nginx-1.15.8/src/core/ngx_md5.c
nginx-1.15.8/src/core/ngx_md5.h
nginx-1.15.8/src/core/ngx_module.c
nginx-1.15.8/src/core/ngx_module.h
nginx-1.15.8/src/core/ngx_murmurhash.c
nginx-1.15.8/src/core/ngx_murmurhash.h
nginx-1.15.8/src/core/ngx_open_file_cache.c
nginx-1.15.8/src/core/ngx_open_file_cache.h
nginx-1.15.8/src/core/ngx_output_chain.c
nginx-1.15.8/src/core/ngx_palloc.c
nginx-1.15.8/src/core/ngx_palloc.h
nginx-1.15.8/src/core/ngx_parse.c
nginx-1.15.8/src/core/ngx_parse.h
nginx-1.15.8/src/core/ngx_parse_time.c
nginx-1.15.8/src/core/ngx_queue.c
nginx-1.15.8/src/core/ngx_parse_time.h
nginx-1.15.8/src/core/ngx_proxy_protocol.c
nginx-1.15.8/src/core/ngx_proxy_protocol.h
nginx-1.15.8/src/core/ngx_queue.h
nginx-1.15.8/src/core/ngx_radix_tree.c
nginx-1.15.8/src/core/ngx_radix_tree.h
nginx-1.15.8/src/core/ngx_rbtree.c
nginx-1.15.8/src/core/ngx_rbtree.h
nginx-1.15.8/src/core/ngx_regex.c
nginx-1.15.8/src/core/ngx_regex.h
nginx-1.15.8/src/core/ngx_resolver.c
nginx-1.15.8/src/core/ngx_resolver.h
nginx-1.15.8/src/core/ngx_rwlock.c
nginx-1.15.8/src/core/ngx_rwlock.h
nginx-1.15.8/src/core/ngx_sha1.c
nginx-1.15.8/src/core/ngx_sha1.h
nginx-1.15.8/src/core/ngx_shmtx.c
nginx-1.15.8/src/core/ngx_shmtx.h
nginx-1.15.8/src/core/ngx_slab.c
nginx-1.15.8/src/core/ngx_slab.h
nginx-1.15.8/src/core/ngx_spinlock.c
nginx-1.15.8/src/core/ngx_string.c
nginx-1.15.8/src/core/ngx_string.h
nginx-1.15.8/src/core/ngx_syslog.c
nginx-1.15.8/src/core/ngx_syslog.h
nginx-1.15.8/src/core/ngx_thread_pool.c
nginx-1.15.8/src/core/ngx_thread_pool.h
nginx-1.15.8/src/core/ngx_times.c
nginx-1.15.8/src/core/ngx_times.h
nginx-1.15.8/contrib/README
nginx-1.15.8/contrib/geo2nginx.pl
nginx-1.15.8/contrib/unicode2nginx/
nginx-1.15.8/contrib/vim/
nginx-1.15.8/contrib/vim/ftdetect/
nginx-1.15.8/contrib/vim/ftplugin/
nginx-1.15.8/contrib/vim/indent/
nginx-1.15.8/contrib/vim/syntax/
nginx-1.15.8/contrib/vim/syntax/nginx.vim
nginx-1.15.8/contrib/vim/indent/nginx.vim
nginx-1.15.8/contrib/vim/ftplugin/nginx.vim
nginx-1.15.8/contrib/vim/ftdetect/nginx.vim
nginx-1.15.8/contrib/unicode2nginx/koi-utf
nginx-1.15.8/contrib/unicode2nginx/unicode-to-nginx.pl
nginx-1.15.8/contrib/unicode2nginx/win-utf
nginx-1.15.8/conf/fastcgi.conf
nginx-1.15.8/conf/fastcgi_params
nginx-1.15.8/conf/koi-utf
nginx-1.15.8/conf/koi-win
nginx-1.15.8/conf/mime.types
nginx-1.15.8/conf/nginx.conf
nginx-1.15.8/conf/scgi_params
nginx-1.15.8/conf/uwsgi_params
nginx-1.15.8/conf/win-utf
nginx-1.15.8/auto/cc/
nginx-1.15.8/auto/define
nginx-1.15.8/auto/endianness
nginx-1.15.8/auto/feature
nginx-1.15.8/auto/have
nginx-1.15.8/auto/have_headers
nginx-1.15.8/auto/headers
nginx-1.15.8/auto/include
nginx-1.15.8/auto/init
nginx-1.15.8/auto/install
nginx-1.15.8/auto/lib/
nginx-1.15.8/auto/make
nginx-1.15.8/auto/module
nginx-1.15.8/auto/modules
nginx-1.15.8/auto/nohave
nginx-1.15.8/auto/options
nginx-1.15.8/auto/os/
nginx-1.15.8/auto/sources
nginx-1.15.8/auto/stubs
nginx-1.15.8/auto/summary
nginx-1.15.8/auto/threads
nginx-1.15.8/auto/types/
nginx-1.15.8/auto/unix
nginx-1.15.8/auto/types/sizeof
nginx-1.15.8/auto/types/typedef
nginx-1.15.8/auto/types/uintptr_t
nginx-1.15.8/auto/types/value
nginx-1.15.8/auto/os/conf
nginx-1.15.8/auto/os/darwin
nginx-1.15.8/auto/os/freebsd
nginx-1.15.8/auto/os/linux
nginx-1.15.8/auto/os/solaris
nginx-1.15.8/auto/os/win32
nginx-1.15.8/auto/lib/conf
nginx-1.15.8/auto/lib/geoip/
nginx-1.15.8/auto/lib/google-perftools/
nginx-1.15.8/auto/lib/libatomic/
nginx-1.15.8/auto/lib/libgd/
nginx-1.15.8/auto/lib/libxslt/
nginx-1.15.8/auto/lib/make
nginx-1.15.8/auto/lib/openssl/
nginx-1.15.8/auto/lib/pcre/
nginx-1.15.8/auto/lib/perl/
nginx-1.15.8/auto/lib/zlib/
nginx-1.15.8/auto/lib/zlib/conf
nginx-1.15.8/auto/lib/zlib/make
nginx-1.15.8/auto/lib/zlib/makefile.bcc
nginx-1.15.8/auto/lib/zlib/makefile.msvc
nginx-1.15.8/auto/lib/zlib/makefile.owc
nginx-1.15.8/auto/lib/perl/conf
nginx-1.15.8/auto/lib/perl/make
nginx-1.15.8/auto/lib/pcre/conf
nginx-1.15.8/auto/lib/pcre/make
nginx-1.15.8/auto/lib/pcre/makefile.bcc
nginx-1.15.8/auto/lib/pcre/makefile.msvc
nginx-1.15.8/auto/lib/pcre/makefile.owc
nginx-1.15.8/auto/lib/openssl/conf
nginx-1.15.8/auto/lib/openssl/make
nginx-1.15.8/auto/lib/openssl/makefile.bcc
nginx-1.15.8/auto/lib/openssl/makefile.msvc
nginx-1.15.8/auto/lib/libxslt/conf
nginx-1.15.8/auto/lib/libgd/conf
nginx-1.15.8/auto/lib/libatomic/conf
nginx-1.15.8/auto/lib/libatomic/make
nginx-1.15.8/auto/lib/google-perftools/conf
nginx-1.15.8/auto/lib/geoip/conf
nginx-1.15.8/auto/cc/acc
nginx-1.15.8/auto/cc/bcc
nginx-1.15.8/auto/cc/ccc
nginx-1.15.8/auto/cc/clang
nginx-1.15.8/auto/cc/conf
nginx-1.15.8/auto/cc/gcc
nginx-1.15.8/auto/cc/icc
nginx-1.15.8/auto/cc/msvc
nginx-1.15.8/auto/cc/name
nginx-1.15.8/auto/cc/owc
nginx-1.15.8/auto/cc/sunc
```

- También descargamos el codigo fuente de las dependencias oblitagorias:

```shell
joseju@joseju:~/nginx/dep$ wget https://www.openssl.org/source/openssl-1.1.1a.tar.gz && tar xzvf openssl-1.1.1a.tar.gz
--2022-11-29 21:28:30--  https://www.openssl.org/source/openssl-1.1.1a.tar.gz
Resolviendo www.openssl.org (www.openssl.org)... 184.25.37.154, 2a02:26f0:dd:497::c1e, 2a02:26f0:dd:49b::c1e
Conectando con www.openssl.org (www.openssl.org)[184.25.37.154]:443... conectado.
Petición HTTP enviada, esperando respuesta... 200 OK
Longitud: 8350547 (8,0M) [application/x-gzip]
Grabando a: «openssl-1.1.1a.tar.gz»

openssl-1.1.1a.tar.gz     100%[=====================================>]   7,96M  2,24MB/s    en 3,7s    

2022-11-29 21:28:35 (2,18 MB/s) - «openssl-1.1.1a.tar.gz» guardado [8350547/8350547]
```

- Accedemos al directorio a compilar y copiamos el manual al directorio /usr/share:

```shell
joseju@joseju:~/nginx$ sudo cp nginx-1.15.8/man/nginx.8 /usr/share/man/man8
joseju@joseju:~/nginx$ sudo gzip /usr/share/man/man8/nginx.8 
joseju@joseju:~/nginx$ ls /usr/share/man/man8 | egrep nginx.8.gz
nginx.8.gz
```

- Hecho esto, creamos el makefile:

```shell
joseju@joseju:~/nginx/nginx-1.15.8$ ./configure --prefix=/etc/nginx \
            --sbin-path=/usr/sbin/nginx \
            --modules-path=/usr/lib/nginx/modules \
            --conf-path=/etc/nginx/nginx.conf \
            --error-log-path=/var/log/nginx/error.log \
            --pid-path=/var/run/nginx.pid \
            --lock-path=/var/run/nginx.lock \
            --user=nginx \
            --group=nginx \
            --build=Debian \
            --builddir=nginx-1.15.8 \
            --with-select_module \
            --with-poll_module \
            --with-threads \
            --with-file-aio \
            --with-http_ssl_module \
            --with-http_v2_module \
            --with-http_realip_module \
            --with-http_addition_module \
            --with-http_xslt_module=dynamic \
            --with-http_image_filter_module=dynamic \
            --with-http_geoip_module=dynamic \
            --with-http_sub_module \
            --with-http_dav_module \
            --with-http_flv_module \
            --with-http_mp4_module \
            --with-http_gunzip_module \
            --with-debugsl-opt=no-nextprotoneg \ \/scgi_temp \ \mp \p \
checking for OS
 + Linux 5.10.0-19-amd64 x86_64
checking for C compiler ... found
 + using GNU C compiler
 + gcc version: 10.2.1 20210110 (Debian 10.2.1-6) 
checking for gcc -pipe switch ... found
checking for -Wl,-E switch ... found
checking for gcc builtin atomic operations ... found
checking for C99 variadic macros ... found
checking for gcc variadic macros ... found
checking for gcc builtin 64 bit byteswap ... found
checking for unistd.h ... found
checking for inttypes.h ... found
checking for limits.h ... found
checking for sys/filio.h ... not found
checking for sys/param.h ... found
checking for sys/mount.h ... found
checking for sys/statvfs.h ... found
checking for crypt.h ... found
checking for Linux specific features
checking for epoll ... found
checking for EPOLLRDHUP ... found
checking for EPOLLEXCLUSIVE ... found
checking for O_PATH ... found
checking for sendfile() ... found
checking for sendfile64() ... found
checking for sys/prctl.h ... found
checking for prctl(PR_SET_DUMPABLE) ... found
checking for prctl(PR_SET_KEEPCAPS) ... found
checking for capabilities ... found
checking for crypt_r() ... found
checking for sys/vfs.h ... found
checking for poll() ... found
checking for /dev/poll ... not found
checking for kqueue ... not found
checking for crypt() ... not found
checking for crypt() in libcrypt ... found
checking for F_READAHEAD ... not found
checking for posix_fadvise() ... found
checking for O_DIRECT ... found
checking for F_NOCACHE ... not found
checking for directio() ... not found
checking for statfs() ... found
checking for statvfs() ... found
checking for dlopen() ... not found
checking for dlopen() in libdl ... found
checking for sched_yield() ... found
checking for sched_setaffinity() ... found
checking for SO_SETFIB ... not found
checking for SO_REUSEPORT ... found
checking for SO_ACCEPTFILTER ... not found
checking for SO_BINDANY ... not found
checking for IP_TRANSPARENT ... found
checking for IP_BINDANY ... not found
checking for IP_BIND_ADDRESS_NO_PORT ... found
checking for IP_RECVDSTADDR ... not found
checking for IP_SENDSRCADDR ... not found
checking for IP_PKTINFO ... found
checking for IPV6_RECVPKTINFO ... found
checking for TCP_DEFER_ACCEPT ... found
checking for TCP_KEEPIDLE ... found
checking for TCP_FASTOPEN ... found
checking for TCP_INFO ... found
checking for accept4() ... found
checking for kqueue AIO support ... not found
checking for Linux AIO support ... found
checking for int size ... 4 bytes
checking for long size ... 8 bytes
checking for long long size ... 8 bytes
checking for void * size ... 8 bytes
checking for uint32_t ... found
checking for uint64_t ... found
checking for sig_atomic_t ... found
checking for sig_atomic_t size ... 4 bytes
checking for socklen_t ... found
checking for in_addr_t ... found
checking for in_port_t ... found
checking for rlim_t ... found
checking for uintptr_t ... uintptr_t found
checking for system byte ordering ... little endian
checking for size_t size ... 8 bytes
checking for off_t size ... 8 bytes
checking for time_t size ... 8 bytes
checking for AF_INET6 ... found
checking for setproctitle() ... not found
checking for pread() ... found
checking for pwrite() ... found
checking for pwritev() ... found
checking for sys_nerr ... found
checking for localtime_r() ... found
checking for clock_gettime(CLOCK_MONOTONIC) ... found
checking for posix_memalign() ... found
checking for memalign() ... found
checking for mmap(MAP_ANON|MAP_SHARED) ... found
checking for mmap("/dev/zero", MAP_SHARED) ... found
checking for System V shared memory ... found
checking for POSIX semaphores ... not found
checking for POSIX semaphores in libpthread ... found
checking for struct msghdr.msg_control ... found
checking for ioctl(FIONBIO) ... found
checking for struct tm.tm_gmtoff ... found
checking for struct dirent.d_namlen ... not found
checking for struct dirent.d_type ... found
checking for sysconf(_SC_NPROCESSORS_ONLN) ... found
checking for sysconf(_SC_LEVEL1_DCACHE_LINESIZE) ... found
checking for openat(), fstatat() ... found
checking for getaddrinfo() ... found
checking for libxslt ... found
checking for libexslt ... found
checking for GD library ... found
checking for GD WebP support ... found
checking for perl
 + perl version: This is perl 5, version 32, subversion 1 (v5.32.1) built for x86_64-linux-gnu-thread-multi
 + perl interpreter multiplicity found
checking for GeoIP library ... found
checking for GeoIP IPv6 support ... found
creating nginx-1.15.8/Makefile

Configuration summary
  + using threads
  + using PCRE library: ../dep/pcre-8.42
  + using OpenSSL library: ../dep/openssl-1.1.1a
  + using zlib library: ../dep/zlib-1.2.13

  nginx path prefix: "/etc/nginx"
  nginx binary file: "/usr/sbin/nginx"
  nginx modules path: "/usr/lib/nginx/modules"
  nginx configuration prefix: "/etc/nginx"
  nginx configuration file: "/etc/nginx/nginx.conf"
  nginx pid file: "/var/run/nginx.pid"
  nginx error log file: "/var/log/nginx/error.log"
  nginx http access log file: "/var/log/nginx/access.log"
  nginx http client request body temporary files: "/var/cache/nginx/client_temp"
  nginx http proxy temporary files: "/var/cache/nginx/proxy_temp"
  nginx http fastcgi temporary files: "/var/cache/nginx/fastcgi_temp"
  nginx http uwsgi temporary files: "/var/cache/nginx/uwsgi_temp"
  nginx http scgi temporary files: "/var/cache/nginx/scgi_temp"
```

- Compilamos el programa:

```shell
joseju@joseju:~/nginx/nginx-1.15.8$ make
make -f nginx-1.15.8/Makefile
make[1]: se entra en el directorio '/home/joseju/nginx/nginx-1.15.8'
cd ../dep/pcre-8.42 \
&& if [ -f Makefile ]; then make distclean; fi \
&& CC="cc" CFLAGS="-O2 -fomit-frame-pointer -pipe " \
./configure --disable-shared  --enable-jit
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /usr/bin/mkdir -p
checking for gawk... gawk
checking whether make sets $(MAKE)... yes
checking whether make supports nested variables... yes
checking whether make supports nested variables... (cached) yes
checking for style of include used by make... GNU
checking for gcc... cc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether cc accepts -g... yes
checking for cc option to accept ISO C89... none needed
checking whether cc understands -c and -o together... yes
checking dependency style of cc... gcc3
checking for ar... ar

joseju@joseju:~/nginx/nginx-1.15.8$ sudo make install
make -f nginx-1.15.8/Makefile install
make[1]: se entra en el directorio '/home/joseju/nginx/nginx-1.15.8'
cd nginx-1.15.8/src/http/modules/perl && make install
make[2]: se entra en el directorio '/home/joseju/nginx/nginx-1.15.8/nginx-1.15.8/src/http/modules/perl'
"/usr/bin/perl" -MExtUtils::Command::MM -e 'cp_nonempty' -- nginx.bs blib/arch/auto/nginx/nginx.bs 644
Manifying 1 pod document
Files found in blib/arch: installing files in blib/lib into architecture dependent library tree
Installing /usr/share/perl/5.24.1/x86_64-linux-gnu-thread-multi/auto/nginx/nginx.so
Installing /usr/share/perl/5.24.1/x86_64-linux-gnu-thread-multi/nginx.pm
Installing /usr/share/perl/5.24.1/man3/nginx.3pm
Appending installation info to /usr/share/perl/5.24.1/x86_64-linux-gnu-thread-multi/perllocal.pod
make[2]: se sale del directorio '/home/joseju/nginx/nginx-1.15.8/nginx-1.15.8/src/http/modules/perl'
test -d '/etc/nginx' || mkdir -p '/etc/nginx'
test -d '/usr/sbin' \
	|| mkdir -p '/usr/sbin'
test ! -f '/usr/sbin/nginx' \
	|| mv '/usr/sbin/nginx' \
		'/usr/sbin/nginx.old'
cp nginx-1.15.8/nginx '/usr/sbin/nginx'
test -d '/etc/nginx' \
	|| mkdir -p '/etc/nginx'
cp conf/koi-win '/etc/nginx'
cp conf/koi-utf '/etc/nginx'
cp conf/win-utf '/etc/nginx'
test -f '/etc/nginx/mime.types' \
	|| cp conf/mime.types '/etc/nginx'
cp conf/mime.types '/etc/nginx/mime.types.default'
test -f '/etc/nginx/fastcgi_params' \
	|| cp conf/fastcgi_params '/etc/nginx'
cp conf/fastcgi_params \
	'/etc/nginx/fastcgi_params.default'
test -f '/etc/nginx/fastcgi.conf' \
	|| cp conf/fastcgi.conf '/etc/nginx'
cp conf/fastcgi.conf '/etc/nginx/fastcgi.conf.default'
test -f '/etc/nginx/uwsgi_params' \
	|| cp conf/uwsgi_params '/etc/nginx'
cp conf/uwsgi_params \
	'/etc/nginx/uwsgi_params.default'
test -f '/etc/nginx/scgi_params' \
	|| cp conf/scgi_params '/etc/nginx'
cp conf/scgi_params \
	'/etc/nginx/scgi_params.default'
test -f '/etc/nginx/nginx.conf' \
	|| cp conf/nginx.conf '/etc/nginx/nginx.conf'
cp conf/nginx.conf '/etc/nginx/nginx.conf.default'
test -d '/var/run' \
	|| mkdir -p '/var/run'
test -d '/var/log/nginx' \
	|| mkdir -p '/var/log/nginx'
test -d '/etc/nginx/html' \
	|| cp -R html '/etc/nginx'
test -d '/var/log/nginx' \
	|| mkdir -p '/var/log/nginx'
test -d '/usr/lib/nginx/modules' \
	|| mkdir -p '/usr/lib/nginx/modules'
test ! -f '/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so' \
		'/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so.old'
cp nginx-1.15.8/ngx_http_xslt_filter_module.so '/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_http_image_filter_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_image_filter_module.so' \
		'/usr/lib/nginx/modules/ngx_http_image_filter_module.so.old'
cp nginx-1.15.8/ngx_http_image_filter_module.so '/usr/lib/nginx/modules/ngx_http_image_filter_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_http_geoip_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_geoip_module.so' \
		'/usr/lib/nginx/modules/ngx_http_geoip_module.so.old'
cp nginx-1.15.8/ngx_http_geoip_module.so '/usr/lib/nginx/modules/ngx_http_geoip_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_http_perl_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_perl_module.so' \
		'/usr/lib/nginx/modules/ngx_http_perl_module.so.old'
cp nginx-1.15.8/ngx_http_perl_module.so '/usr/lib/nginx/modules/ngx_http_perl_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_stream_geoip_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_stream_geoip_module.so' \
		'/usr/lib/nginx/modules/ngx_stream_geoip_module.so.old'
cp nginx-1.15.8/ngx_stream_geoip_module.so '/usr/lib/nginx/modules/ngx_stream_geoip_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_mail_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_mail_module.so' \
		'/usr/lib/nginx/modules/ngx_mail_module.so.old'
cp nginx-1.15.8/ngx_mail_module.so '/usr/lib/nginx/modules/ngx_mail_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_stream_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_stream_module.so' \
		'/usr/lib/nginx/modules/ngx_stream_module.so.old'
cp nginx-1.15.8/ngx_stream_module.so '/usr/lib/nginx/modules/ngx_stream_module.so'
make[1]: se sale del directorio '/home/joseju/nginx/nginx-1.15.8'
```

- Instalamos:

```shell
joseju@joseju:~/nginx/nginx-1.15.8$ sudo make install
make -f nginx-1.15.8/Makefile install
make[1]: se entra en el directorio '/home/joseju/nginx/nginx-1.15.8'
cd nginx-1.15.8/src/http/modules/perl && make install
make[2]: se entra en el directorio '/home/joseju/nginx/nginx-1.15.8/nginx-1.15.8/src/http/modules/perl'
"/usr/bin/perl" -MExtUtils::Command::MM -e 'cp_nonempty' -- nginx.bs blib/arch/auto/nginx/nginx.bs 644
Manifying 1 pod document
Files found in blib/arch: installing files in blib/lib into architecture dependent library tree
Installing /usr/share/perl/5.24.1/x86_64-linux-gnu-thread-multi/auto/nginx/nginx.so
Installing /usr/share/perl/5.24.1/x86_64-linux-gnu-thread-multi/nginx.pm
Installing /usr/share/perl/5.24.1/man3/nginx.3pm
Appending installation info to /usr/share/perl/5.24.1/x86_64-linux-gnu-thread-multi/perllocal.pod
make[2]: se sale del directorio '/home/joseju/nginx/nginx-1.15.8/nginx-1.15.8/src/http/modules/perl'
test -d '/etc/nginx' || mkdir -p '/etc/nginx'
test -d '/usr/sbin' \
	|| mkdir -p '/usr/sbin'
test ! -f '/usr/sbin/nginx' \
	|| mv '/usr/sbin/nginx' \
		'/usr/sbin/nginx.old'
cp nginx-1.15.8/nginx '/usr/sbin/nginx'
test -d '/etc/nginx' \
	|| mkdir -p '/etc/nginx'
cp conf/koi-win '/etc/nginx'
cp conf/koi-utf '/etc/nginx'
cp conf/win-utf '/etc/nginx'
test -f '/etc/nginx/mime.types' \
	|| cp conf/mime.types '/etc/nginx'
cp conf/mime.types '/etc/nginx/mime.types.default'
test -f '/etc/nginx/fastcgi_params' \
	|| cp conf/fastcgi_params '/etc/nginx'
cp conf/fastcgi_params \
	'/etc/nginx/fastcgi_params.default'
test -f '/etc/nginx/fastcgi.conf' \
	|| cp conf/fastcgi.conf '/etc/nginx'
cp conf/fastcgi.conf '/etc/nginx/fastcgi.conf.default'
test -f '/etc/nginx/uwsgi_params' \
	|| cp conf/uwsgi_params '/etc/nginx'
cp conf/uwsgi_params \
	'/etc/nginx/uwsgi_params.default'
test -f '/etc/nginx/scgi_params' \
	|| cp conf/scgi_params '/etc/nginx'
cp conf/scgi_params \
	'/etc/nginx/scgi_params.default'
test -f '/etc/nginx/nginx.conf' \
	|| cp conf/nginx.conf '/etc/nginx/nginx.conf'
cp conf/nginx.conf '/etc/nginx/nginx.conf.default'
test -d '/var/run' \
	|| mkdir -p '/var/run'
test -d '/var/log/nginx' \
	|| mkdir -p '/var/log/nginx'
test -d '/etc/nginx/html' \
	|| cp -R html '/etc/nginx'
test -d '/var/log/nginx' \
	|| mkdir -p '/var/log/nginx'
test -d '/usr/lib/nginx/modules' \
	|| mkdir -p '/usr/lib/nginx/modules'
test ! -f '/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so' \
		'/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so.old'
cp nginx-1.15.8/ngx_http_xslt_filter_module.so '/usr/lib/nginx/modules/ngx_http_xslt_filter_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_http_image_filter_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_image_filter_module.so' \
		'/usr/lib/nginx/modules/ngx_http_image_filter_module.so.old'
cp nginx-1.15.8/ngx_http_image_filter_module.so '/usr/lib/nginx/modules/ngx_http_image_filter_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_http_geoip_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_geoip_module.so' \
		'/usr/lib/nginx/modules/ngx_http_geoip_module.so.old'
cp nginx-1.15.8/ngx_http_geoip_module.so '/usr/lib/nginx/modules/ngx_http_geoip_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_http_perl_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_http_perl_module.so' \
		'/usr/lib/nginx/modules/ngx_http_perl_module.so.old'
cp nginx-1.15.8/ngx_http_perl_module.so '/usr/lib/nginx/modules/ngx_http_perl_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_stream_geoip_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_stream_geoip_module.so' \
		'/usr/lib/nginx/modules/ngx_stream_geoip_module.so.old'
cp nginx-1.15.8/ngx_stream_geoip_module.so '/usr/lib/nginx/modules/ngx_stream_geoip_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_mail_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_mail_module.so' \
		'/usr/lib/nginx/modules/ngx_mail_module.so.old'
cp nginx-1.15.8/ngx_mail_module.so '/usr/lib/nginx/modules/ngx_mail_module.so'
test ! -f '/usr/lib/nginx/modules/ngx_stream_module.so' \
	|| mv '/usr/lib/nginx/modules/ngx_stream_module.so' \
		'/usr/lib/nginx/modules/ngx_stream_module.so.old'
cp nginx-1.15.8/ngx_stream_module.so '/usr/lib/nginx/modules/ngx_stream_module.so'
make[1]: se sale del directorio '/home/joseju/nginx/nginx-1.15.8'
```

- Comprobamos la versión de nginx para ver si se ha instalado:

```shell
joseju@joseju:~$ sudo nginx -V
nginx version: nginx/1.15.8 (Debian)
built by gcc 10.2.1 20210110 (Debian 10.2.1-6) 
built with OpenSSL 1.1.1a  20 Nov 2018
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --user=nginx --group=nginx --build=Debian --builddir=nginx-1.15.8 --with-select_module --with-poll_module --with-threads --with-file-aio --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_addition_module --with-http_xslt_module=dynamic --with-http_image_filter_module=dynamic --with-http_geoip_module=dynamic --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module --with-http_secure_link_module --with-http_degradation_module --with-http_slice_module --with-http_stub_status_module --with-http_perl_module=dynamic --with-perl_modules_path=/usr/share/perl/5.24.1 --with-perl=/usr/bin/perl --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --with-mail=dynamic --with-mail_ssl_module --with-stream=dynamic --with-stream_ssl_module --with-stream_realip_module --with-stream_geoip_module=dynamic --with-stream_ssl_preread_module --with-compat --with-pcre=../dep/pcre-8.42 --with-pcre-jit --with-zlib=../dep/zlib-1.2.13 --with-openssl=../dep/openssl-1.1.1a --with-openssl-opt=no-nextprotoneg --with-debug
```

### Desisntalación

Para realizar la desistalación hacemos lo siguiente:

- Elimininamos el directorio donde está instalado y el manual:

```shell
joseju@joseju:~/nginx$ ls
dep  nginx-1.15.8
joseju@joseju:~/nginx$ rm -fr *
joseju@joseju:~/nginx$ ls
joseju@joseju:~/nginx$ sudo rm /usr/share/man/man8/nginx.8.gz
```

- Comprobamos la versión del paquete para ver si está eliminado: 

```shell
joseju@joseju:~$ sudo nginx -V
sudo: nginx: command not found
```