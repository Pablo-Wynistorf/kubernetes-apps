Create the metric service with this command:



```
kubectl apply -f https://raw.githubusercontent.com/Pablo-Wynistorf/kubernetes-apps/main/metrics-server/metrics-server.yaml
```

Please be aware that the metrics-server image version might not be up to date in the mentioned kubectl file. Please check the current image version [here](https://github.com/kubernetes-sigs/metrics-server/releases). If the version is not up to date anymore, download the file and make the changes by hand. The kubectl file can be downloaded [here](https://github.com/Pablo-Wynistorf/kubernetes-apps/blob/main/metrics-server/metrics-server.yaml).

![](https://slabstatic.com/prod/uploads/ptzfq7y2/posts/images/6QrqHO5iNQZm9RPwqTkl8PQJ.png)



Check if it works:

```
kubectl get ds metrics-server -n kube-system
```



Watch metrics from pods and nodes with:

```
kubectl top nodes

kubectl top pods -n namespace
```
