# Description

Includes required files for deployment of project kubeedge-app in k8s environments.

The manifests required for deployment are included in [folder](manifests). Includes:

- Workload deployment description, kubeedge-app-deployment.yml
- Workload service, kubeedge-app-service.yml

It also includes the the manifest required to register it in argo-cd automatically.

- kubeedge-project.yml
- kubeedge-app.yml

Creating project
```
kubectl apply -f kubeedge-project.yml -n argo-cd
```


Creating application
```
kubectl apply -f kubeedge-app.yml -n argo-cd
```

## Notes

Since the target of the deployments are edge nodes is important to define the target of such deployments using available affinity labels:

```yaml
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                - key: node-role.kubernetes.io/edge
                  operator: Exists
                - key: node-role.kubernetes.io/agent
                  operator: Exists  
```

In this case it looks for tags present in edge nodes.


Checking node labels

```
kubectl get nodes -owide
```

