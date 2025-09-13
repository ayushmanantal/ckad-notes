# Practice Questions

1. Create a pod with a resource limit of 512 CPU and 1 GiB memory.
2. Create a service account, my-service, and assign it to a pod my-pod running with Redis image.
3. Taint node with `key=size` and `value=small` with NoSchedule taint-effect. Now create a pod and ensure it's running in the same node.
4. Label your primary node with `size=large` and create a pod with node affinity type `requiredDuringSchedulingIgnoredDuringExecution`.
5. Create a multi-container pod with `nginx` and `busybox` containers.
6. Create a pod with two containers. First will be an initContainer that will use the busybox image and execute the shell command `sleep 1200`. For another container, use the Nginx image.