# Overall workout based on helm

Example: creating helm package from helm create, then installing package and checking all insfrastrucuture and eventually deleting all.

```sh
# After creating nginx-helm then package it 

➜  helm git:(main) ✗ helm package nginx-helm
Successfully packaged chart and saved it to: /Users/maheshbogati/Desktop/kind-k8s-cluster/helm/nginx-helm-0.1.0.tgz


➜  helm git:(main) ✗ helm install dev-nginx-service nginx-helm
NAME: dev-nginx-service
LAST DEPLOYED: Sat Apr 26 18:39:59 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/name=nginx-helm,app.kubernetes.io/instance=dev-nginx-service" -o jsonpath="{.items[0].metadata.name}")
  export CONTAINER_PORT=$(kubectl get pod --namespace default $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl --namespace default port-forward $POD_NAME 8080:$CONTAINER_PORT
➜  helm git:(main) ✗ kubectl get pods
NAME                                            READY   STATUS    RESTARTS   AGE
dev-nginx-service-nginx-helm-6dc5f595f4-rwdtx   1/1     Running   0          54s
dev-nginx-service-nginx-helm-6dc5f595f4-s65hf   1/1     Running   0          54s
➜  helm git:(main) ✗ kubectl get svc
NAME                           TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
dev-nginx-service-nginx-helm   ClusterIP   10.96.67.39   <none>        80/TCP    63s
kubernetes                     ClusterIP   10.96.0.1     <none>        443/TCP   3d18h
➜  helm git:(main) ✗ kubectl get deployment
NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
dev-nginx-service-nginx-helm   2/2     2            2           73s
➜  helm git:(main) ✗ kubectl get all
NAME                                                READY   STATUS    RESTARTS   AGE
pod/dev-nginx-service-nginx-helm-6dc5f595f4-rwdtx   1/1     Running   0          80s
pod/dev-nginx-service-nginx-helm-6dc5f595f4-s65hf   1/1     Running   0          80s

NAME                                   TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/dev-nginx-service-nginx-helm   ClusterIP   10.96.67.39   <none>        80/TCP    80s
service/kubernetes                     ClusterIP   10.96.0.1     <none>        443/TCP   3d18h

NAME                                           READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dev-nginx-service-nginx-helm   2/2     2            2           80s

NAME                                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/dev-nginx-service-nginx-helm-6dc5f595f4   2         2         2       80s
➜  helm git:(main) ✗ 


# deleting helm package and checking 

➜  helm git:(main) ✗ helm uninstall dev-nginx-service
release "dev-nginx-service" uninstalled
➜  helm git:(main) ✗ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   3d18h
➜  helm git:(main) ✗ 


```
