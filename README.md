# minikube-helm-jenkins

Verify minikube is running:
```
$ minikube status
minikube: Running
cluster: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100
```


Create namespace:
```
$ kubectl create -f minikube/jenkins-namespace.yaml
```

Create storageclass
```
$ kubectl create -f minikube/jenkins-storageclass.yaml
```

Create persistent volume (folder /data is persistent on minikube)
```
$ kubectl create -f minikube/jenkins-volume.yaml
```

Create persistent volume claim
```
$ kubectl create -f minikube/jenkins-pvc.yaml
```


Execute helm:
```
$ helm install --name jenkins -f helm/jenkins-values.yaml stable/jenkins --namespace jenkins-project
```


Check admin password for jenkins:
```
$ printf $(kubectl get secret --namespace jenkins-project jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

clusterrolebinding
~~~
kubectl create clusterrolebinding permissive-binding --clusterrole=cluster-admin --user=admin --user=kubelet --group=system:serviceaccounts
~~~
Full tutorial can be found [here](https://medium.com/@lvthillo/deploy-jenkins-with-dynamic-slaves-in-minikube-8aef5404e9c1).
