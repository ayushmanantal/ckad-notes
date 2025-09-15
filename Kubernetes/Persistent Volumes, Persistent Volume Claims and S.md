# Persistent Storage in Kubernetes

This guide covers three key components of Kubernetes storage management:

- Persistent Volumes (PV): Cluster-wide storage resources
- Persistent Volume Claims (PVC): Storage requests by users
- Storage Classes: Dynamic storage provisioning

## Overview

### Persistent Volumes (PV)

PVs are cluster resources that provide persistent storage, independent of pod lifecycle.
📚 [Official PV Documentation](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

### Persistent Volume Claims (PVC)

PVCs are requests for storage by users, which can be fulfilled by PVs.
📚 [Official PVC Documentation](https://kubernetes.io/docs/concepts/storage/persistent-volume-claims/)

### Storage Classes

Enable dynamic provisioning of PVs based on storage requirements.
📚 [Official Storage Class Documentation](https://kubernetes.io/docs/concepts/storage/storage-classes/)

## Storage Configuration Examples

### 1. Creating a Persistent Volume

Create a persistent volume with specific capacity and access modes:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
spec:
  persistentVolumeReclaimPolicy: Retain # Keep data when PVC is deleted
  accessModes:
    - ReadWriteMany # Multiple nodes can read and write
  capacity:
    storage: 100Mi # Volume size
  hostPath:
    path: /pv/log # Path on the host machine
```

### 2. Creating a Persistent Volume Claim

Request storage resources from available PVs:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
    - ReadWriteOnce # Single node read/write access
  resources:
    requests:
      storage: 50Mi # Requested storage size
```

### 3. Using PVC in a Pod

Mount the claimed storage in a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
    - name: event-simulator
      image: kodekloud/event-simulator
      env:
        - name: LOG_HANDLERS
          value: file
      volumeMounts:
        - mountPath: /log # Mount point in container
          name: log-volume # Reference to volume

  volumes:
    - name: log-volume
      persistentVolumeClaim:
        claimName: claim-log-1 # Reference to PVC
```

### 4. Configuring a Storage Class

Define dynamic storage provisioning:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: delayed-volume-sc
provisioner: kubernetes.io/no-provisioner # Storage provisioner
volumeBindingMode: WaitForFirstConsumer # Delay binding until pod using PVC is created
```

## Access Modes

PVs and PVCs support three access modes:

1. `ReadWriteOnce (RWO)`: Volume can be mounted as read-write by a single node
2. `ReadOnlyMany (ROX)`: Volume can be mounted read-only by many nodes
3. `ReadWriteMany (RWX)`: Volume can be mounted as read-write by many nodes

## Reclaim Policies

PVs can have different reclaim policies:

- `Retain`: Manual reclamation
- `Delete`: Automatic deletion
- `Recycle`: Basic scrub (deprecated)

## Best Practices

1. 🔄 Use StorageClasses for dynamic provisioning
2. 📊 Set appropriate resource requests
3. 🔒 Configure correct access modes
4. 💾 Plan for data backup and recovery
5. 📝 Document storage requirements

## Additional Resources

### Official Documentation

- 📚 [Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
- 📚 [Storage Classes](https://kubernetes.io/docs/concepts/storage/storage-classes/)
- 📚 [Dynamic Volume Provisioning](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/)

### Storage Management

- 📚 [Volume Snapshots](https://kubernetes.io/docs/concepts/storage/volume-snapshots/)
- 📚 [Storage Capacity](https://kubernetes.io/docs/concepts/storage/storage-capacity/)
- 📚 [Volume Health Monitoring](https://kubernetes.io/docs/concepts/storage/volume-health-monitoring/)
