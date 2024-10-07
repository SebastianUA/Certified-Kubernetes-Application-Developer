# CKAD Mock Exam: Jobs and CronJobs

## Task 1: Create a Job to Perform a Task
Create a Kubernetes **Job** that runs a pod to print the numbers from 1 to 5, with a 1-second delay between each number.

### Requirements:
- Use the `busybox` image.
- The command inside the container should print numbers from 1 to 5, with a 1-second delay between each number.
- The Job should terminate successfully after the command completes.

### Solution:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: number-job
spec:
  template:
    spec:
      containers:
      - name: number-container
        image: busybox
        command: ["sh", "-c", "for i in $(seq 1 5); do echo $i; sleep 1; done"]
      restartPolicy: Never
  backoffLimit: 3
```

## Task 2: Create a Parallel Job
Create a **parallel Job** that runs 3 pods, each performing the same task of printing "Processing" and the current pod's name.

### Requirements:
- Use the `busybox` image.
- Each pod should print "Processing in pod: `<pod_name>`".
- The Job should run 3 parallel pods.

### Solution:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: parallel-job
spec:
  parallelism: 3
  completions: 3
  template:
    spec:
      containers:
      - name: parallel-container
        image: busybox
        command: ["sh", "-c", "echo Processing in pod: $(hostname)"]
      restartPolicy: Never
  backoffLimit: 3
```

## Task 3: Create a CronJob
Create a **CronJob** that runs every 5 minutes, displaying the current time in the pod's logs.

### Requirements:
- Use the `busybox` image.
- The CronJob should run every 5 minutes.
- The container should print the current date and time using the `date` command.

### Solution:
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: cronjob-print-time
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: time-container
            image: busybox
            command: ["date"]
          restartPolicy: Never
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 2
```

## Task 4: Clean Up Failed Jobs from a CronJob
Modify the **CronJob** created in Task 3 to keep only the last 2 failed jobs in its history.

### Requirements:
- The CronJob should already be configured.
- Limit the number of failed jobs to 2.

### Solution:
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: cronjob-print-time
spec:
  schedule: "*/5 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: time-container
            image: busybox
            command: ["date"]
          restartPolicy: Never
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 2
```

## Task 5: Troubleshoot a Failing Job
You have a **Job** named `failing-job` that is stuck in `CrashLoopBackOff`. Investigate why the Job is failing and resolve the issue.

### Steps:
1. Run `kubectl describe job failing-job` and inspect the pod status for any errors.
2. Check the logs of the pods with `kubectl logs <pod-name>`.
3. If the image is incorrect or the command is misconfigured, edit the Job and correct it.

### Example:
```bash
kubectl describe job failing-job
kubectl logs <failing-pod-name>
kubectl edit job failing-job
```

Possible issues:
- Incorrect image.
- Incorrect command or arguments.
- Missing permissions or resources.