
# Kubernetes CronJobs

## Overview
CronJobs in Kubernetes are used to manage time-based jobs, scheduling the execution of jobs at specified times or intervals. They are useful for periodic and recurring tasks, such as backups, report generation, and sending emails.

## CronJob YAML Example
Here’s an example of a CronJob manifest:
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: example-cronjob
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: example-container
            image: busybox
            command: ["sh", "-c", "date; echo Hello from the Kubernetes cluster"]
          restartPolicy: OnFailure
```
This manifest creates a CronJob named `example-cronjob` that runs a container using the `busybox` image. The container executes a simple command every 5 minutes to print the date and a message.

## Common Commands

### Creating and Managing CronJobs
- **Create a CronJob**:
  ```bash
  kubectl create -f cronjob-ex.yml  # create CronJob from a file
  ```

- **List CronJobs**:
  ```bash
  kubectl get cronjobs
  ```

### Managing CronJob Executions
- **List the Jobs spawned by a CronJob**:
  ```bash
  kubectl get jobs --watch
  ```

- **Delete a CronJob**:
  ```bash
  kubectl delete cronjob <cronjob-name>
  ```

## Summary
CronJobs are essential for automating regular tasks within a Kubernetes environment, providing reliability and flexibility in how jobs are scheduled and managed.
