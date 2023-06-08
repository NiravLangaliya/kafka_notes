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







**Senior Data Engineer**

**Company: [Your Company Name]**

**Location: [Your Company Location or "Remote"]**

**Job Type: [Full-Time / Part-Time / Contract]**

We are seeking an experienced and passionate Senior Data Engineer to join our dynamic and forward-thinking team. In this role, you will leverage your deep understanding of data processing technologies and programming languages to develop and optimize our data infrastructure. 

**Key Responsibilities:**

1. Perform large-scale data processing using Python and Apache Spark.
2. Utilize AWS services like S3, EMR, EC2 for data storage and processing.
3. Schedule, monitor, and troubleshoot data pipelines with Apache Airflow.
4. Ensure the data quality, observability, and integrity across all our data infrastructure.
5. Implement data structures and algorithms to optimize data processing and pipelines.
6. Write and maintain robust Python test cases to validate data processing tasks.
7. Oversee data versioning, backups, and restoration to ensure data resilience and availability.
8. Collaborate with cross-functional teams to implement best practices in data management.

**Skills & Qualifications:**

1. Bachelors or Master’s degree in Computer Science, Data Science, or related field.
2. Proven experience in a Data Engineering role, with a focus on large-scale data processing.
3. Strong knowledge of Python programming language.
4. Proficiency with AWS services, including S3, EMR, and EC2.
5. Hands-on experience with Apache Spark and Apache Airflow.
6. Excellent understanding of data structures and algorithms.
7. Experience in writing Python test cases to validate data processing tasks.
8. Deep understanding of data quality assurance, data versioning, backups, and restoration.
9. Familiarity with Git or Bitbucket for version control.
10. Strong communication and collaboration skills.

**What We Offer:**

[Provide details about the benefits and perks you offer. This could include health insurance, retirement plans, paid time off, flexible working hours, and so on.]

If you're a proactive, problem-solving individual with a passion for big data, we'd love to hear from you. Apply today to join our team and take the next step in your data engineering career.

[Your Company Name] is proud to be an equal opportunity employer.

Please note that this job description is not designed to cover or contain a comprehensive listing of activities, duties, or responsibilities that are required of the employee for this job. Duties, responsibilities, and activities may change at any time with or without notice.
