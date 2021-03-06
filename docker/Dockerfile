
FROM debian:buster-slim

USER root

RUN apt-get update -qq && apt install -y --no-install-recommends \
      curl git python3-pip chromium-driver apache2 libapache2-mod-wsgi-py3 \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && apt-get clean -y

RUN pip3 install flask flask-cors selenium setuptools wheel

WORKDIR /var/
RUN mkdir /var/sbgn-rest-renderer
COPY api /var/sbgn-rest-renderer/api
COPY sbgnrender /var/sbgn-rest-renderer/sbgnrender
COPY LICENSE.md scripts/apache_config.sh setup.py /var/sbgn-rest-renderer/

WORKDIR /var/sbgn-rest-renderer
RUN cd sbgnrender \
   && rm -fr node_modules/* app/bundle.js package-lock.json \
   && npm install \
   && npm run build-bundle-js
   
RUN pip3 install .

RUN a2enmod wsgi \
   && /bin/sh apache_config.sh \
   && a2dissite 000-default \
   && a2ensite sbgn_rest

RUN mkdir static \
   && chown -R www-data:www-data static \
   && echo '{"status": "true"}' > static/status

EXPOSE 80

# CMD apachectl -D FOREGROUND
CMD bash -c "service apache2 start & tail -f /dev/null"
