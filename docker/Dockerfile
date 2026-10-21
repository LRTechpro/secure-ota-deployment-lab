# Lightweight NGINX serves the partner's static HTML, CSS, and JavaScript files.
FROM nginx:alpine

# Only the website assets required at runtime are copied into the image.
COPY index.html /usr/share/nginx/html/index.html
COPY styles.css /usr/share/nginx/html/styles.css
COPY firmware.js /usr/share/nginx/html/firmware.js
COPY app.js /usr/share/nginx/html/app.js
COPY scripts /usr/share/nginx/html/scripts

# NGINX accepts HTTP traffic on port 80 inside this delivery image.
EXPOSE 80

# Docker reports the container unhealthy if the website stops responding.
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -q --spider http://127.0.0.1/ || exit 1
