FROM docker.io/node:9.9.0-alpine

EXPOSE 8081

RUN apk add --no-cache git

ADD https://api.github.com/repos/kkenan/basic-microservices/contents/node-app /dev/null
RUN git clone https://github.com/kkenan/basic-microservices.git /tmp/basic-microservices

USER node

WORKDIR /home/node/

RUN cp /tmp/basic-microservices/node-app/package*.json /home/node/

RUN npm install

RUN cp /tmp/basic-microservices/node-app/* /home/node/ && \
	chmod 555 -R /home/node/

ENTRYPOINT ["node","index.js"]