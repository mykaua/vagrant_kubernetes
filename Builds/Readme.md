#Kubernetes command used:


1) kubectl get nodes -> list nodes
2) kubectl get pods -> list pods
3) kubectl describe nodes -> information about nodes
4) kubectl describe pod -> information about pods

  Simple Pods:
5)kubectl create -f /path/to/yaml/file -> create pods:
  - kubectl create -f nginx.yml  --> example

6) kubectl get pods -o wide -> more information about pods
7) kubectl get pods --show-all --> show all nodes(live and dead)
8) kubectl run busybox --image=busybox --restart=Never --tty -i --generator=run-pod/v1 -> run busybox Pod (pod should on the same node):
  - wget -qO- http://172.17.0.2  -> check nginx
9)kubectl delete pod busybox --> delete pod (deleted busybox pod)
10) kubectl port-forward nginx :80 &  -> Listen on port any on the local machine and forward to port 80 on my-pod,symbol "&" exit from tty
  - [root@master Builds]# kubectl port-forward nginx :80 &
    [1] 3750
    [root@master Builds]# Forwarding from 127.0.0.1:35461 -> 80
    Forwarding from [::1]:35461 -> 80
    wget -qO- http://localhost:35461 --> check if nginx available

  - kubectl port-forward nginx 8081:80 &  --> forward port to 8081
    wget -qO- http://localhost:8081

  Simple pod with labels/tags
11) kubectl create -f nginx-pod-label.yml
12) kubectl get pods -l app=nginx -> show pods with nginx label (app-key,nginx-value)
    - [root@master Simple Pod]# kubectl get pods -l app=nginx2
      NAME      READY     STATUS    RESTARTS   AGE
      nginx2    1/1       Running   0          55s
      [root@master Simple Pod]# kubectl get pods -l app=nginx
      NAME      READY     STATUS    RESTARTS   AGE
      nginx     1/1       Running   0          6m
      nginx3    1/1       Running   0          49s
      [root@master Simple Pod]#
13) kubectl describe pods -l app=nginx -> information about pods with label nginx

  Deployment Pod
14) kubectl create -f nginx-deployment.yml
15) kubectl get pods
    - nginx-deployment-prod-2430912311-49krd   1/1       Running   0          50s  --> id has been added to name
16) kubectl get deployment -> List a particular deployment
    - NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
      nginx-deployment-prod   1         1         1            1           2m
17) kubectl get deploy --> Create another deployment
    - NAME                    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    nginx-deployment-dev    1         1         1            1           10s
    nginx-deployment-prod   1         1         1            1           6m
18) kubectl describe deployments -l app=nginx-deployment-dev --> get information about deployment
19) kubectl apply -f nginx-deploy-dev-update.yml --> apply/update changes to deploy(changed vesrions of nginx from latest to 1.8)

  Replcation Pod
20) kubectl create -f nginx-multi-label.yml
21) kubecl get pods
    - NAME              READY     STATUS    RESTARTS   AGE
    nginx-www-lfc83   1/1       Running   0          2m
    nginx-www-mctqh   1/1       Running   0          2m
    nginx-www-vv7mt   1/1       Running   0          2m
21) kubectl describe replicationcontroller -> information about replications
22) kubectl get services --> list services
  - kubectl get services
  NAME         CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
  kubernetes   10.254.0.1   <none>        443/TCP   20h
23)  kubectl get replicationcontroller --> show replications
24) kubectl delete replicationcontroller nginx-www --> delete replications pods


  Service
25) kubecl create -f nginx-service.yml -> create service
26) kubectl get services --> list services
  - NAME            CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
  kubernetes      10.254.0.1      <none>        443/TCP    20h
  nginx-service   10.254.175.11   <none>        8081/TCP   28s
27) kubectl describe service nginx-service --> information about service
28) kubectl delete service nginx-service --> delete service

  Create temporary pod at command line
29)kubectl run mysample --image=nginx --> created a simple pod, it will create deployment pod
30) kubectl run myreplicas --image=nginx --replicas=2 --labels=app=mynginx,version=1 - create a simple pod with replicas

  Interacting with Pods
31) kubectl exec nginx date --> show date of pod
32) kubectl exec nginx -i -t sh --> enter to pod with shell

  AutoScalling and Scalling pods
33) kubectl run myautoscale --image=nginx --port=80 --labels=app=myautoscale -> create a simple pod
34) kubectl autoscale deployment myautoscale --min=2 --max=5  -> autoscale deployment pod, there can set cpu and ram
35)  kubectl scale --current-replicas=2 --replicas=4 deployment/myautoscale  -> scale manually
36) kubectl get pods -l app=myautoscale --> check pods
    - NAME                           READY     STATUS    RESTARTS   AGE
    myautoscale-3958947512-35hhm   1/1       Running   0          5m
    myautoscale-3958947512-jz27q   1/1       Running   0          3m
    myautoscale-3958947512-n9713   1/1       Running   0          54s
    myautoscale-3958947512-t6zt7   1/1       Running   0          54s

37) kubectl scale --current-replicas=4 --replicas=2 deployment/myautoscale --> downgrade scale
    - kubectl get deployment
    NAME          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    myautoscale   2         2         2            2           7m
