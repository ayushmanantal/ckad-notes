# Jobs and CronJobs in Kubernetes

📚 [Official Documentation: Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
📚 [Official Documentation: CronJobs](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

## Overview

Jobs create pods that run until successful completion of a task. CronJobs create Jobs on a repeating schedule, similar to cron tasks in Linux.

## Jobs

### Types of Jobs

1. **Non-parallel Jobs**

   - Single pod runs until completion
   - Most basic job type

2. **Parallel Jobs with Fixed Completion Count**

   - Multiple pods run in parallel
   - Job completes when specified number of pods succeed

3. **Parallel Jobs with Work Queue**
   - Multiple pods process items from a queue
   - Job completes when queue is empty

### Basic Job Example

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: data-processor
spec:
  template:
    spec:
      containers:
        - name: processor
          image: data-processor:v1
          command: ["python", "process.py"]
      restartPolicy: OnFailure
```

### Creating Jobs

````bash
# Create job from command line
kubectl create job my-job --image=busybox -- echo "Hello Job"

# Create job definition (dry-run)
kubectl create job data-processor --image=processor:v1 --dry-run=client -o yaml > job.yaml

### Job with Parallelism and Completions
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: parallel-processor
spec:
  completions: 5        # Total number of successful pods
  parallelism: 2        # Number of pods running in parallel
  backoffLimit: 4       # Number of retries before job is marked failed
  template:
    spec:
      containers:
      - name: processor
        image: data-processor:v1
      restartPolicy: OnFailure
````

### Job Patterns

1. **Queue with Fixed Work**

```yaml
spec:
  completions: 10 # Process 10 items
  parallelism: 3 # 3 workers at a time
```

2. **Queue with Size-Based Completion**

```yaml
spec:
  parallelism: 5 # 5 workers
  template:
    spec:
      containers:
        - name: queue-worker
          image: queue-worker:v1
          command: ["process-queue", "--until-empty"]
```

## CronJobs

### Basic CronJob

Runs a job on a time-based schedule:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup-job
spec:
  schedule: "0 2 * * *" # Every day at 2 AM
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: backup
              image: backup-tool:v1
          restartPolicy: OnFailure
```

### Advanced CronJob Settings

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: advanced-job
spec:
  schedule: "*/10 * * * *" # Every 10 minutes
  startingDeadlineSeconds: 60 # Must start within 1 minute
  concurrencyPolicy: Replace # Replace existing job
  successfulJobsHistoryLimit: 3 # Keep 3 successful jobs
  failedJobsHistoryLimit: 1 # Keep 1 failed job
  jobTemplate:
    spec:
      activeDeadlineSeconds: 600 # Job timeout: 10 minutes
      backoffLimit: 3 # Retry 3 times
      template:
        spec:
          containers:
            - name: task
              image: task-runner:v1
          restartPolicy: OnFailure
```

## Common Configurations

### 1. Cron Schedule Syntax

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of the week (0 - 6)
│ │ │ │ │
* * * * *
```

Examples:

- `"0 * * * *"` - Every hour
- `"*/15 * * * *"` - Every 15 minutes
- `"0 0 * * 0"` - Every Sunday midnight
- `"30 20 * * 1-5"` - Weekdays at 8:30 PM

### 2. Concurrency Policies

```yaml
spec:
  concurrencyPolicy: Allow    # Default: allows concurrent jobs
  # or
  concurrencyPolicy: Forbid   # Skip new job if previous still running
  # or
  concurrencyPolicy: Replace  # Cancel old job in favor of new one
```

## Best Practices

1. 🎯 **Job Design**

   - Set appropriate completions/parallelism
   - Use reasonable timeout values
   - Implement idempotency

2. ⏰ **CronJob Timing**

   - Avoid overlapping schedules
   - Set realistic deadlines
   - Consider time zones

3. 🔄 **Retry Handling**

   - Set appropriate backoffLimit
   - Use proper restart policy
   - Handle failures gracefully

4. 📊 **Resource Management**
   - Set resource requests/limits
   - Monitor job duration
   - Clean up completed jobs

## Troubleshooting Commands

```bash
# List all jobs
kubectl get jobs

# List all cronjobs
kubectl get cronjobs

# View job details
kubectl describe job <job-name>

# View cronjob logs
kubectl logs job/<job-name>

# Delete a job
kubectl delete job <job-name>

# Suspend a cronjob
kubectl patch cronjob <name> -p '{"spec":{"suspend":true}}'
```

## Additional Resources

### Official Documentation

- 📚 [Running Automated Tasks with CronJob](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)
- 📚 [Parallel Processing with Jobs](https://kubernetes.io/docs/tasks/job/parallel-processing-expansion/)
- 📚 [Job Patterns](https://kubernetes.io/docs/concepts/workloads/controllers/job/#job-patterns)

### Troubleshooting

- 📚 [Debug Jobs](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/)
- 📚 [CronJob Limitations](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/#cron-job-limitations)
