# Kubernetes — Hands-On in a Real Cluster

Working notes from learning Kubernetes by doing it against a real cluster built with kops on AWS — every object typed out, applied, inspected, and several of them deliberately broken first. Nothing here was read off a blog and pasted in.

## The cluster

`Handon_K8s_objects.txt` starts from zero: install `kubectl` and `kops`, then stand up the cluster:

```bash
kops create cluster --name=kubevpro.groophy.in \
  --state=s3://vproile-kops-state \
  --zones=us-west-2a,us-west-2b \
  --node-count=2 --node-size=t3.small \
  --master-size=t3.medium \
  --dns-zone=kubevpro.groophy.in \
  --node-volume-size=8 --master-volume-size=8
kops update cluster --name kubevpro.groophy.in --yes --admin
```

Two `t3.small` workers across two AZs, S3 for cluster state, Route53 for the DNS zone. The transcript then walks node inspection the long way — `get nodes -o wide`, `-o yaml`, `-o json`, `describe node` — because knowing what a node object looks like precedes trusting it.

## The objects

| File | Topic |
|---|---|
| `Handon_K8s_objects.txt` | The full session transcript (cluster build → pods → inspection) |
| `vproapppod.yaml` | Pod definition for the vProfile app (container on `:8080`) |
| `replicaSet.txt`, `vproapp-repl-controller.yaml` | ReplicationController / ReplicaSets keeping the app alive |
| `deployment.txt` | Deployments and rollouts |
| `vproapp-nodeport.yaml`, `vproapp-loadbalancer.yml` | Service types: NodePort vs LoadBalancer, on the same app |
| `ingress.txt` | Ingress routing rules |
| `configMap.txt`, `secret.txt` | Config and secrets, decoupled from images |
| `volume.txt` | Volumes and persistence |
| `namespaces.txt` | Isolation |
| `CommandAndArguments.txt` | `command` vs `args` in container specs |
| `kubeconfig.txt`, `kopscreate.txt` | Cluster access + kops provisioning |

## The debugging drill — `FixFaultyPod.txt`

The best part of the repo. A pod manifest is created with a deliberately broken image name:

```
image: nginox:1.14.2        # wrong on purpose
```

Then the diagnosis path, in order: `get pod` (see the status), `-o wide` (which node), `-o yaml` (what the cluster actually got), `describe pod` (the events — where the real error lives), `logs` for crash-loops. Fix the typo, re-apply. Then a second faulty pod with bad command arguments, same drill. This is the reflex I'd want on call: describe before you guess.

## The app these objects serve

Every object here (pod, replication controller, both service types) assembles into the same vProfile application that the [Jenkins-CI-CD](https://github.com/amanvicky/Jenkins-CI-CD) pipeline builds and ships — the pipeline produces the image, this cluster is where it would land. The web tier of that app is containerized in [Docker](https://github.com/amanvicky/Docker).
