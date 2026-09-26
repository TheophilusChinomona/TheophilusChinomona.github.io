FROM nginx:alpine

COPY wp-content /usr/share/nginx/html/wp-content
COPY wp-includes /usr/share/nginx/html/wp-includes
COPY s /usr/share/nginx/html/s
COPY *.html /usr/share/nginx/html/
COPY google-fonts.css /usr/share/nginx/html/
COPY deploy/nginx.conf /etc/nginx/conf.d/default.conf
