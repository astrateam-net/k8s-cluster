# Kubernetes Cluster Debugging Guide

## Common Issues and Solutions

### 1. Architecture Mismatch (exec format error)

**Problem**: Pods crash with `exec /nfsplugin: exec format error` or similar errors.

**Root Cause**: Images are downloaded with wrong architecture (e.g., ARM instead of AMD64).

**Solution**:
1. Find the correct digest for AMD64:
```bash
# For nfsplugin example:
curl -sL -H "Accept: application/vnd.docker.distribution.manifest.list.v2+json" \
  https://registry.k8s.io/v2/sig-storage/nfsplugin/manifests/v4.12.1 | \
  python3 -c "import sys, json; data = json.load(sys.stdin); amd64 = [m for m in data['manifests'] if m['platform']['architecture'] == 'amd64'][0]; print(amd64['digest'])"
```

2. Update HelmRelease with specific digest:
```yaml
values:
  nfs:  # or appropriate section
    image:
      repository: registry.k8s.io/sig-storage/nfsplugin
      tag: v4.12.1@sha256:8835d96f4389cb1ea8996b4bf9cad8af960f52508c9b8372a877c6656fd92264
```

3. Delete incorrect images on all nodes:
```bash
# See section "Debug Nodes"
```

### 2. Spegel Cache Issues

**Problem**: Spegel serves wrong image digests even after clearing.

**Solution**:
```bash
# Delete all Spegel pods to clear cache
kubectl delete pod -n kube-system -l app=spegel

# Wait for new pods to restart
kubectl get pods -n kube-system | grep spegel
```

## Debugging Methods

### Launch Debug Pods on All Nodes

**Quick Method**:
```bash
# Apply debug pods to all nodes
kubectl apply -f kubernetes/temp/debug-all-nodes.yaml

# Wait for pods to start
kubectl get pods | grep debug
```

**Manual Method**:
```bash
for node in k8s-01 k8s-02 k8s-03; do
  kubectl run debug-$node --image=raesene/alpine-containertools:latest \
    --node-selector kubernetes.io/hostname=$node \
    --restart=Never \
    --overrides='{
      "spec": {
        "hostNetwork": true,
        "containers": [{
          "name": "debugger",
          "image": "raesene/alpine-containertools:latest",
          "command": ["sleep", "3600"],
          "env": [{"name": "CONTAINER_RUNTIME_ENDPOINT", "value": "unix:///host/run/containerd/containerd.sock"}],
          "securityContext": {"privileged": true},
          "volumeMounts": [{"name": "sock", "mountPath": "/host/run/containerd"}]
        }],
        "volumes": [{"name": "sock", "hostPath": {"path": "/run/containerd", "type": "Directory"}}]
      }
    }'
done
```

### Common Debug Commands

**List container images on a node**:
```bash
# Replace debug-k8s-01 with appropriate pod name
kubectl exec debug-k8s-01 -- crictl images
```

**Find and remove specific images**:
```bash
# List images containing "sig-storage"
kubectl exec debug-k8s-01 -- crictl images | grep sig-storage

# Remove specific image by ID
kubectl exec debug-k8s-01 -- crictl rmi <image-id>

# Remove all images matching pattern
kubectl exec debug-k8s-01 -- sh -c 'crictl images | grep sig-storage | awk "{print \$3}" | xargs -r crictl rmi'
```

**Check image architecture**:
```bash
# Check manifest architecture
kubectl exec debug-k8s-01 -- crictl inspecti <image-id> | grep architecture
```

**Remove images on all nodes**:
```bash
for pod in debug-k8s-01 debug-k8s-02 debug-k8s-03; do
  echo "=== $pod ==="
  kubectl exec $pod -- sh -c 'crictl images | grep <pattern>'
  kubectl exec $pod -- sh -c 'crictl images | grep <pattern> | awk "{print \$3}" | xargs -r crictl rmi'
done
```

## Using the debug-node Taskfile Task

```bash
# Open interactive debug shell on a specific node
task talos:debug-node NODE=k8s-01

# Inside the debug shell, you can use:
# - crictl images              # List images
# - crictl rmi <image-id>      # Remove image
# - crictl rmi --prune         # Remove all unused images
# - uname -m                   # Check node architecture
```

## When to Use Specific Image Digests

Always specify image digests when:
1. Cluster has nodes with different architectures (AMD64, ARM, etc.)
2. Spegel serves wrong images from cache
3. Image repository has multi-architecture manifests
4. You encounter "exec format error"

**How to find correct digest**:
1. Query the registry for manifest list
2. Filter for target architecture (usually amd64)
3. Extract the digest
4. Append to image tag: `image:tag@sha256:digest`

## Troubleshooting Checklist

- [ ] Check pod logs for error messages
- [ ] Verify pod is running on correct node
- [ ] Check image digest matches expected architecture
- [ ] Clear Spegel cache if serving wrong images
- [ ] Remove incorrect images from all nodes
- [ ] Verify HelmRelease values specify correct image with digest
- [ ] Check node architecture matches image architecture
- [ ] Restart affected pods after image fixes
