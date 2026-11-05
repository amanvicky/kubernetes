# Kubernetes — Hands-On Notes

Working notes from learning Kubernetes by doing — every object typed out and broken in a real cluster, not copied.

## Contents

| File | Topic |
|---|---|
| `Handon_K8s_objects.txt` | Core objects walkthrough |
| `vproapppod.yaml` | Pods (incl. a multi-container vProfile app) |
| `replicaSet.txt`, `vproapp-repl-controller.yaml` | ReplicationController / ReplicaSets |
| `deployment.txt` | Deployments & rollouts |
| `vproapp-nodeport.yaml`, `vproapp-loadbalancer.yml` | Service types: NodePort, LoadBalancer |
| `ingress.txt` | Ingress routing |
| `configMap.txt`, `secret.txt` | Configuration & secrets |
| `volume.txt` | Volumes & persistence |
| `namespaces.txt` | Namespace isolation |
| `CommandAndArguments.txt` | CMD vs args in containers |
| `FixFaultyPod.txt` | Debugging broken pods |
| `kubeconfig.txt`, `kopscreate.txt` | Cluster access + kops provisioning |

## Applied in

These objects assemble into the full vProfile CI/CD stack — the same app deployed end-to-end in the [Jenkins-CI-CD](https://github.com/amanvicky/Jenkins-CI-CD) pipeline project.
