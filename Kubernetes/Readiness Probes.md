# Container Probes in Kubernetes

Kubernetes uses probes to monitor container health and manage application lifecycle. These probes help ensure reliable application deployments and runtime behavior.

📚 [Official Documentation: Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

## Overview

### Types of Probes

1. **Readiness Probe**: Determines when a container is ready to accept traffic. If it fails, the pod is removed from service endpoints.
2. **Liveness Probe**: Checks if a container is healthy and running. If it fails, the container is restarted.
3. **Startup Probe**: Indicates when an application has fully started. Disables other probes until successful.

## Probe Mechanisms

### 1. HTTP GET Probe

- Makes HTTP GET request to specified path
- Success: Response code 200-399
- Ideal for web applications and REST services
- Common health check endpoints: `/health`, `/ready`, `/status`

### 2. TCP Socket Probe

- Attempts TCP connection to specified port
- Success: Connection established
- Perfect for database and cache services
- Checks only port availability

### 3. Exec Probe

- Executes command inside container
- Success: Exit code 0
- Flexible for custom health checks
- Example: Checking file existence or custom scripts
  - Indicates if container is ready to serve requests
  - Controls when a pod receives traffic through services
  - Failed probe removes pod from service endpoints

2. **Liveness Probe**:

   - Determines if container is running properly
   - Failed probe triggers container restart
   - Helps recover from deadlocks

3. **Startup Probe** (Not shown in examples):
   - Indicates if container has started successfully
   - Disables liveness and readiness checks until successful
   - Useful for slow-starting containers

### Probe Methods

- HTTP GET: Perform HTTP request
- TCP Socket: Try TCP connection
- Exec: Execute command in container

## Configuration Examples

### 1. Basic Readiness Probe

Using HTTP GET probe to check application readiness:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-2
spec:
  containers:
    - name: simple-webapp-2
      image: kodekloud/webapp-delayed-start
      ports:
        - containerPort: 8080
          protocol: TCP
      readinessProbe:
        httpGet: # Probe type: HTTP GET request
          path: /ready # Endpoint to check
          port: 8080 # Port to access
```

### 2. Liveness Probe with Parameters

Checking container health with custom timing:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-2
spec:
  containers:
    - name: simple-webapp-2
      image: kodekloud/webapp-delayed-start
      ports:
        - containerPort: 8080
          protocol: TCP
      livenessProbe:
        httpGet:
          path: /live # Health check endpoint
          port: 8080 # Port to access
        initialDelaySeconds: 80 # Wait before first probe
        periodSeconds: 1 # How often to probe
```

### 3. TCP Socket Probe Example

Check if port is accepting connections:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tcp-probe
spec:
  containers:
    - name: redis
      image: redis
      ports:
        - containerPort: 6379
      readinessProbe:
        tcpSocket:
          port: 6379 # Port to check
        initialDelaySeconds: 5
        periodSeconds: 10
```

### 4. Exec Probe Example

Execute command inside container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: exec-probe
spec:
  containers:
    - name: app
      image: myapp
      readinessProbe:
        exec:
          command: # Command to execute
            - cat
            - /tmp/healthy
        initialDelaySeconds: 5
        periodSeconds: 5
```

## Probe Parameters

Common configuration options:

```yaml
probe:
  initialDelaySeconds: 15 # Wait before first probe
  periodSeconds: 10 # Time between probes
  timeoutSeconds: 5 # Probe timeout
  successThreshold: 1 # Minimum consecutive successes
  failureThreshold: 3 # Failures before giving up
```

## Best Practices

1. 🎯 Use appropriate probe for each use case

   - Readiness: Service availability
   - Liveness: Application health
   - Startup: Initial boot process

2. 🔄 Configure proper timing

   - Set realistic initialDelaySeconds
   - Avoid too frequent probes
   - Consider application characteristics

3. 🏗️ Design health endpoints

   - Light and fast responses
   - Check critical dependencies
   - Avoid side effects

4. ⚠️ Handle failure scenarios

   - Set appropriate failureThreshold
   - Plan for dependency failures
   - Log probe failures

5. 📊 Monitor probe metrics
   - Track success/failure rates
   - Watch for patterns
   - Adjust based on data

## Additional Resources

### Official Documentation

- 📚 [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
- 📚 [Container Probes](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)
- 📚 [Configure Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

### Health Checking

- 📚 [Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)
- 📚 [Debug Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-pods/)
