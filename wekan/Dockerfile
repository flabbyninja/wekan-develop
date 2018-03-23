FROM flabbyninja/nodebase:latest

ENV MONGO_URL='mongodb://wekan-mongo:27017/wekan'
ENV ROOT_URL='https://example.com'
ENV MAIL_URL='smtp://user:pass@mailserver.example.com:25/'
ENV MAIL_FROM='wekan-admin@example.com'
ENV PORT=8080

RUN npm install node-gyp -g
RUN npm install fibers@2.0 -g --unsafe
RUN curl -sL https://install.meteor.com | bash
RUN mkdir ~/repos
RUN cd ~/repos && git clone https://github.com/wekan/wekan.git
RUN cd ~/repos/wekan && git checkout devel && npm install

RUN cd ~/repos/wekan \
    && mkdir -p ~/repos/wekan/packages \
    && cd ~/repos/wekan/packages \
    && git clone https://github.com/wekan/flow-router.git kadira-flow-router \
    && git clone https://github.com/meteor-useraccounts/core.git meteor-useraccounts-core \
    && sed -i 's/api\.versionsFrom/\/\/api.versionsFrom/' ~/repos/wekan/packages/meteor-useraccounts-core/package.js \
    && cd ~/repos/wekan \
    && rm -rf node_modules \
    && meteor npm install \
    && rm -rf .build \
    && meteor build .build --directory --allow-superuser \
    && cp -f fix-download-unicode/cfs_access-point.txt .build/bundle/programs/server/packages/cfs_access-point.js \
    && cd ~/repos/wekan/.build/bundle/programs/server/npm/node_modules/meteor/npm-bcrypt \
    && rm -rf node_modules/bcrypt \
    && meteor npm install bcrypt \
    && cd ~/repos/wekan/.build/bundle/programs/server \
    && rm -rf node_modules \
    && meteor npm install \
    && meteor npm install bcrypt