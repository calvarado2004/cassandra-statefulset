# cassandra-statefulset

You should have Portworx installed on your kubernetes cluster first.

```
$ kubectl create namespace cassandra
$ kubectl apply -f kubernetes.yaml
$ kubectl get all -n cassandra                                        
NAME              READY   STATUS    RESTARTS   AGE
pod/cassandra-0   1/1     Running   0          7m17s
pod/cassandra-1   1/1     Running   0          6m32s
pod/cassandra-2   1/1     Running   0          4m49s

NAME                TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
service/cassandra   ClusterIP   None         <none>        9042/TCP   7m17s

NAME                         READY   AGE
statefulset.apps/cassandra   3/3     7m17s
```
