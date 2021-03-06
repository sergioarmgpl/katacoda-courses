There is a sample HelloWorld function written in and can be inspected.

`export HELLO_SRC=https://raw.githubusercontent.com/nuclio/nuclio/master/hack/examples/golang/helloworld/helloworld.go`{{execute}}

`curl $HELLO_SRC`{{execute}}

Deploy the function to Nuclio on Kubernetes. This will involve building the code from a base Go language container and pushing the to container with the Hello World Go code into the private registry.

`nuctl deploy helloworld -n nuclio -p $HELLO_SRC --registry $REGISTRY`{{execute}}

Inspect the contents and see the new container has been deployed.

`curl $REGISTRY/v2/_catalog | jq`{{execute}}

Execute the new function.

`nuctl invoke -n nuclio helloworld`{{execute}}

Go back to the portal and inspect and run it from there as well. In the dashboard select the _nuclio_ namespace from the drop-down and click on the _default_ project. There you will see the _helloworld_ function.

# Your Sandbox

You can use this Katacoda scenario to experiment with many more features available in Nuclio such as:

- Pluggable triggers such as HTTP, Kafka, MQTT, RabbitMQ, Kinesis, and more
- Stream processing
- Connecting to volumes
- Develop functions in different languages
- Scaling
- Function versioning and deployment model
- Debugging

More [examples are here](https://github.com/nuclio/nuclio/blob/development/hack/examples/README.md)
