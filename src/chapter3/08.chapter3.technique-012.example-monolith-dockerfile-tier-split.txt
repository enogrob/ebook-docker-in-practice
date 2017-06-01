FROM ubuntu:14.04
RUN apt-get update && apt-get install postgresql nodejs npm nginx
WORKDIR /opt
COPY db /opt/db                                     -+
RUN service postgresql start && \                    |- db setup
    cat db/schema.sql | psql && \                    |
    service postgresql stop                         -+
COPY app /opt/app                                   -+
RUN cd app && npm install                            |- app setup
RUN cd app && ./minify_static.sh                    -+
COPY conf /opt/conf                                 -+
RUN cp conf/mysite /etc/nginx/sites-available/ && \  +
    cd /etc/nginx/sites-enabled && \                 |- web setup
    ln -s ../sites-available/mysite                 -+
