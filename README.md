# kafka_notes

Here's an example of how to configure a liveness probe for a Kafka broker running in a Kubernetes pod using a TCP socket check:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kafka-broker
  labels:
    app: kafka-broker
spec:
  replicas: 3
  selector:
    matchLabels:
      app: kafka-broker
  template:
    metadata:
      labels:
        app: kafka-broker
    spec:
      containers:
        - name: kafka-broker
          image: my-kafka-image:latest
          ports:
            - name: tcp
              containerPort: 9092
          livenessProbe:
            tcpSocket:
              port: tcp
            initialDelaySeconds: 15
            periodSeconds: 10
            failureThreshold: 3
          env:
            - name: KAFKA_BROKER_ID
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name

```
In this example, the livenessProbe field is added to the Kafka broker container spec. The tcpSocket check specifies that Kubernetes should open a TCP connection to port 9092 on the container to determine if the Kafka broker is alive and healthy. The initialDelaySeconds field indicates how many seconds Kubernetes should wait before performing the first liveness check, and the periodSeconds field specifies how often Kubernetes should check the liveness of the container.

The failureThreshold field indicates the number of consecutive failures that must occur before the container is considered unhealthy and should be restarted. In this case, if the liveness check fails three times in a row, Kubernetes will automatically restart the container.

Note that this example assumes that the Kafka broker container is exposing port 9092 for client connections. If the broker is using a different port or a custom protocol, you will need to modify the livenessProbe accordingly.
