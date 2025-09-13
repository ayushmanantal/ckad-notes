# Jobs and CronJobs

### Creating a Job

```bash
kubectl create job throw-dice-job --image=kodekloud/throw-dice --dry-run=client -o yaml > throw-dice-job.yaml
```

### Definition CronJob

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: throw-dice-cron-job
spec:
  schedule: "30 21 * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: throw-dice-cron-job
            image: kodekloud/throw-dice
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

### With Completions and Parallelism

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: throw-dice-cron-job
spec:
  schedule: "30 21 * * *"
  jobTemplate:
    spec:
      completions: 3
      parallelism: 3
      backoffLimit: 25 # This is so the job does not quit before it succeeds.
      template:
        spec:
          containers:
          - name: throw-dice
            image: kodekloud/throw-dice
          restartPolicy: Never
```

### With Starting Deadline Settings

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello-cron-job
spec:
  schedule: "*/1 * * * *"
  startingDeadlineSeconds: 17
  concurrencyPolicy: Replace
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            command:
            - /bin/sh
            - -c
            - "date; echo Hello from the Kubernetes cluster"
          restartPolicy: OnFailure

```