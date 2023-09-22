link to install k8's dashboard 
https://www.learnitguide.net/2020/10/install-kubernetes-dashboard-deploy.html

**Steps to install Dashboard in k8's and access to by token**

1. **Install Kubernetes Dashboard**  
a. Kubernetes stored their recommended yaml files for Dashboard in github repository. Use kubectl command to deploy the kubernetes dashboard as below.  
b. from here will get latest yaml file https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/  
Ex of install dashboard by yaml file
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

2. **How to Access Kubernetes Dashboard?**
when you want to access your kubernetes dashboard from outside the cluster then we must expose the service using LoadBalancer or NodePort type. Since default service type is Cluster IP.

![image](https://github.com/soumya-devops/kubernetes/assets/37827483/cde7fa83-d36b-4022-96b2-78bfbb98280a)
