link to install k8's dashboard 
https://www.learnitguide.net/2020/10/install-kubernetes-dashboard-deploy.html

**Steps to install Dashboard in k8's and access to by token**

1. **Install Kubernetes Dashboard**
2. Kubernetes stored their recommended yaml files for Dashboard in github repository. Use kubectl command to deploy the kubernetes dashboard as below.
3. from here will get latest yaml file https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
Ex of install dashboard by yaml file
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
