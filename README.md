# How to build and add it to Dojot

Build the docker image:
```sh
docker build -t <your dockerHub username>/decoder .
```

Publish it on your DockerHub:
```sh
docker push <your dockerHub username>/decoder
```

Acquire a Dojot's token:
```sh
JWT=$(curl -s -X POST http://localhost:8000/auth \
-H 'Content-Type:application/json' \
-d '{"username": "admin", "passwd" : "admin"}' | jq -r ".jwt")
```

Note: the previous command requires the `jq` command, you can install it on ubuntu
with the following command:
```
sudo apt-get install jq
```

Add the node to Dojot.
```sh
curl -H "Authorization: Bearer ${JWT}" http://localhost:8000/flows/v1/node -H 'content-type: application/json' -d '{"image": "<your dockerHub username>/decoder", "id":"decoder"}'
```

Now the decoder node will be available on `converters` category into the FlowBroker Dojot's interface.

Note: the DockerHub use is optional, you can use a private docker registry instead.

Note2: All commands considers that you are running Dojot locally, if it is not
the case, please, adapt them to refect your scenario.