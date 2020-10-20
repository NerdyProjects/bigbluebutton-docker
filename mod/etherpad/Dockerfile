FROM node:12-alpine

RUN addgroup -g 5001 etherpad && adduser -D -u 5001 -G etherpad etherpad \
    && apk add git \
    && chown etherpad:0 /opt

USER etherpad

# clone specific git commit
ENV COMMIT 96e7ea5e54e4752aabb12c5476745ce124c71f4c
RUN cd /opt \
    && git clone --depth=1 https://github.com/bigbluebutton/etherpad-lite.git \
    && cd /opt/etherpad-lite \
    && git fetch --depth 1 origin $COMMIT \
    && git checkout $COMMIT

WORKDIR /opt/etherpad-lite

# install node dependencies for Etherpad
RUN bin/installDeps.sh && \
	rm -rf ~/.npm/_cacache

# install plugins
RUN npm install ep_delete_after_delay_lite  git+https://git@github.com/pedrobmarin/ep_redis_publisher.git

COPY --chown=etherpad:0 settings.json /opt/etherpad-lite/settings.json
COPY entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]