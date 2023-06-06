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


No, Kubernetes does not always perform liveness checks on TCP.

Liveness probes are used by Kubernetes to determine if a container in a pod is still running and healthy. The probes can be configured to check for different aspects of the container's health, including whether it is listening on a particular port or responding to HTTP requests.

While it is common to use HTTP endpoints for liveness probes, Kubernetes also supports TCP probes, which can be used to check if a container is listening on a specific TCP port. However, the use of TCP probes is not mandatory, and it ultimately depends on how the container is configured and what kind of health checks are necessary for it.

To pass the Airflow context to a decorator that is decorating a task function in Apache Airflow, you can use the `@functools.wraps` decorator from the Python standard library. Here's an example of how you can achieve this:

```python
import functools

def my_decorator(task_func):
    @functools.wraps(task_func)
    def wrapper(*args, **kwargs):
        # Access the Airflow context using the `get_current_context` method
        context = kwargs['ti'].get_task_instance().get_template_context()

        # Your decorator logic here, using the `context` variable

        return task_func(*args, **kwargs)

    return wrapper


@task_decorator
def my_task():
    # Your task logic here
    pass
```

In this example, the `my_decorator` function is a decorator that wraps the `task_func` function. The `@functools.wraps` decorator ensures that the wrapped function retains its original name and docstring.

Inside the `wrapper` function, the `kwargs` parameter contains the keyword arguments passed to the task function, which includes the `ti` (task instance) object. By accessing `kwargs['ti']`, you can obtain the task instance, and then use the `get_template_context` method to get the Airflow context.

You can then use the `context` variable within your decorator logic as needed. Finally, the original `task_func` is called with the appropriate arguments and the result is returned.

Note that this approach assumes you are using the `PythonOperator` to define your task. If you're using a different operator or mechanism to define tasks in Airflow, the approach may vary slightly.

**<img width="732" alt="image" src="https://github.com/NiravLangaliya/kafka_notes/assets/13895686/bd7748e7-dd27-4c8a-bc8d-c4f88a1c0325">
**
While it is common to use HTTP endpoints for liveness probes, Kubernetes also supports TCP probes, which can be used to check if a container is listening on a specific TCP port. However, the use of TCP probes is not mandatory, and it ultimately depends on how the container is configured and what kind of health checks are necessary for it.
