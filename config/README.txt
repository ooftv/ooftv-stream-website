Nginx expects nginx.conf here:
/usr/local/etc/nginx/


Need to symlink from
/usr/local/etc/nginx/

to this folder

ln -s /path/to/github/version /path/to/link

ln -sf /usr/local/bin/ooftv-stream-website/config/nginx /usr/local/etc/nginx
