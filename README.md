# About

Given the problem of [**Task 4**](https://github.com/dojot/dojot-training), step 2.

A company produces a device with pollutants and oxygenation level. It will install several of these devices along a river near a factory to check the water treatment. **Due to the simplicity of device, it sends the data corresponding to the two measurements in a 16-bit message, each reading being encoded in 8 bits**. Data is sent every hour to a gateway that relays them to dojot via HTTP.

## Problem: 

Given the limitation of the devices, it will be necessary to convert from 16 bits to 8 bits, separating the messages.

That way, we can use Dojot's [Flow Broker](https://github.com/dojot/flowbroker) to perform this task whenever new data arrives. **However, that knot does not yet exist!**
For the task, you will need to create a new node that will do this conversion.

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