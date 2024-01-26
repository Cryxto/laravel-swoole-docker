FROM nginx:stable-alpine

ENV TZ="Asia/Jakarta"
ENV PS1="\u@\h:\w\\$ "

RUN apk add --no-cache bash
RUN apk add --no-cache tzdata
RUN apk add --no-cache net-tools
RUN apk add --no-cache busybox-extras
# RUN setsebool -P httpd_can_network_connect 1