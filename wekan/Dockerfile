FROM flabbyninja/nodebase:latest

RUN npm install node-gyp -g
RUN npm install fibers@2.0 -g --unsafe

# meteor installer doesn't work with the default tar binary
RUN apt-get install -y bsdtar \
    && cp $(which tar) $(which tar)~ \
    && ln -sf $(which bsdtar) $(which tar)

# install Meteor forcing its progress
RUN curl "https://install.meteor.com/?release=1.6.0.1" \
    | sed 's/VERBOSITY="--silent"/VERBOSITY="--progress-bar"/' \
    | sh

# put back the original tar
RUN mv $(which tar)~ $(which tar)

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

ENV MONGO_URL='mongodb://mongo:27017/wekan'

CMD cd ~/repos/wekan \
    && meteor run --allow-superuser