## Dapr in action

### Installation

1. Install dapr
    ```
    brew install dapr/tap/dapr-cli
    ```

2. Initialize dapr (to start dapr control plane)
    ```
    dapr init
    ```
    *Note: `dapr init` deploys a redis container and Zipkin distributed tracing system*

3. Check all components (dapr, redis and zipkin)
      ```
      docker ps
      ```

### Run pub/sub application

1. Install publisher dependencies
    ```
    (cd publisher && pip3 install -r requirements.txt)
    ```
2. Install subscriber dependencies
    ```
    (cd subscriber && pip3 install -r requirements.txt)
    ```
3. Start pub/sub applications
    ```
    dapr run -f dapr.yaml
    ```
### Test interchangeable components

1. Stop the pub/sub applications `ctrl + c`
2. In the `components/pubsub.yaml`, comment out the redis component and uncomment the rabbitmq
3. Deploy rabbitmq
    ```
    docker run -d --hostname my-rabbit \
      --name some-rabbit \
      -p 5672:5672 -p 15672:15672 \
      rabbitmq:3
    ```
4. Run pub/sub applications
    ```
    dapr run -f dapr.yaml
    ```
5. (Optional) Check requests traces with zipkin, open `localhost:9411` in the browser and click `Run Query`
