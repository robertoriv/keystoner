FROM node

MAINTAINER Roberto Rivera <roberto.rivera@gmail.com>

# copy the app files to the /app folder
COPY . /app

WORKDIR /app

# grab all dependencies
RUN npm install

# start the app
CMD ["node", "keystone"]