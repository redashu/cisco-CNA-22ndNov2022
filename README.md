# cisco-CNA-22ndNov2022

## traning plan 

<img src="plan.png">

### revision step 1 

<img src="rev1.png">

### REv step 2 

<img src="rev2.png">

### k8s networking 

<img src="k8snet.png">

## Deploy app and implement Ingress rule 

### deploying app with UI 

```
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl  create  deployment  ashu-app  --image=docker.io/library/adminer:latest --port 8080 --dry-run=client -o yaml >day
4app.yaml 
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl  apply -f day4app.yaml 
deployment.apps/ashu-app created
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl  get  deploy 
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
ashu-app   1/1     1            1           5s
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl  get  po 
NAME                      READY   STATUS    RESTARTS   AGE
ashu-app-f6cc5cd7-sptv6   1/1     Running   0          9s
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl  get  po -o wide
NAME                      READY   STATUS    RESTARTS   AGE   IP                NODE      NOMINATED NODE   READINESS GATES
ashu-app-f6cc5cd7-sptv6   1/1     Running   0          14s   192.168.235.143   worker1   <none>           <none>
[ashu@ip-172-31-16-246 ashu-container-apps]$ 
```

## Creating cluster IP type service in k8s 

```
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl   get  deploy 
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
ankit-app1   1/1     1            1           22m
ashu-app     1/1     1            1           27m
atul-app     1/1     1            1           26m
leni-app     1/1     1            1           22m
mkj-app      1/1     1            1           25m
teju-app     1/1     1            1           24m
vijay-app    1/1     1            1           23m
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl  expose deployment  ashu-app  --type ClusterIP --port 8080 --name ashu-app-lb --dry-run=client -o yaml >applb.yaml
[ashu@ip-172-31-16-246 ashu-container-apps]$ ls
admin.conf  adminer.yaml  applb.yaml  ashu-front-end.yaml  day4app.yaml  dbsvc.yaml  db.yaml  load_bal.yaml  one-page-website-html-css-project  project-html-website
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl apply -f applb.yaml 
service/ashu-app-lb created
[ashu@ip-172-31-16-246 ashu-container-apps]$ kubectl   get  service 
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
ashu-app-lb   ClusterIP   10.97.239.123   <none>        8080/TCP   5s
kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP    49m
[ashu@ip-172-31-16-246 ashu-container-apps]$ 
```

### ingress rule yaml 

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ashu-app-route-rule # name of rule 
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx # class name of Ingress -- example , nginx , haproxy , istio
  rules:
  - host: me.ashutoshh.in # domain name to check by ingress rule 
    http:
      paths:
      - path: / # home page means / 
        pathType: Prefix
        backend:
          service:
            name: ashu-app-lb # name of service 
            port:
              number: 8080 # internal service name 
```



