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
In below image you can see service type is Cluster IP  

![image](https://github.com/soumya-devops/kubernetes/assets/37827483/cde7fa83-d36b-4022-96b2-78bfbb98280a)  

Find the service type and change from ClusterIP to LoadBalancer, save and exit from the file. 
```
kubectl get svc -n kubernetes-dashboard
```
```
kubectl -n kubernetes-dashboard edit svc kubernetes-dashboard
```
Make sure the service type changed to LoadBalancer.  
```
kubectl -n kubernetes-dashboard get svc
```
Get the ELB address from the output.  (Access it with **https://**)  
![image](https://github.com/soumya-devops/kubernetes/assets/37827483/761cd491-07ee-46e3-8820-248e51463d84)  
You can access your dashboard from any browser using the ELB address you have got.

3. **How to get Login Credentials to access Kubernetes Dashboard?**  
There are two ways you can login into the dashboard.

a. Kubeconfig

b. Token  
Let me use token method, that is the recommended login method. For that we need to create cluster admin service account.  
  1. **Create a Service Account:**  
     Kubernetes Dashboard uses a Service Account for authentication. You can create one with the following YAML manifest:  
     As k8s 1.24 version didn't create secret automatically for service-account , need to reate it manually.
     
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
secrets:
  - name: admin-token-secret
---
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: admin-token-secret
  namespace: kubernetes-dashboard
  annotations:
    kubernetes.io/service-account.name: "admin-user"
```  
Save this YAML to a file, e.g., dashboard-adminuser.yaml, and apply it:

```
 kubectl apply -f dashboard-adminuser.yaml
 ```
Commands to check service account and secrets
```
kubectl get serviceaccount -n kubernetes-dashboard
kubectl describe serviceaccount admin-user -n kubernetes-dashboard
kubectl describe secret admin-token-secret -n kubernetes-dashboard --> get token here
```

2. **Create a Cluster Role Binding:**
To grant the necessary permissions to the Service Account, you can create a Cluster Role Binding. Save the following YAML to a file, e.g., dashboard-clusterrolebinding.yaml, and apply it:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user-binding
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
```
Apply it using: 
```
kubectl apply -f dashboard-clusterrolebinding.yaml
```
3. **Get the Bearer Token:**
To get the token for the admin-user Service Account, you can use the following command:
```
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```
