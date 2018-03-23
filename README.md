MongoDB 

docker run --name wekan-mongo -d mongo

Base app

docker run --privileged=true --link wekan-mongo:mongo -p 3000:3000 -it -v wekan-source:/wekan-source flabbyninja/wekandev:latest bash