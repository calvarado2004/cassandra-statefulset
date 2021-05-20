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

You can insert example data

```

$ kubectl exec -it pod/cassandra-0 -n cassandra -- cqlsh                                            
Connected to K8Demo at 127.0.0.1:9042.
[cqlsh 5.0.1 | Cassandra 3.11.10 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cqlsh> CREATE KEYSPACE IF NOT EXISTS example_db WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 3, 'datacenter2': 2 };
cqlsh> describe keyspaces

system_schema  system      system_traces
system_auth    example_db  system_distributed
cqlsh> USE example_db;
cqlsh:example_db> CREATE TABLE example_db.cyclist_name (id text PRIMARY KEY, lastname text, firstname text );
cqlsh:example_db> INSERT INTO cyclist_name (id, lastname, firstname) VALUES (c4b65263-fe58-4846-83e8-f0e1c13d518f,'Alvarado','Carlos');
cqlsh:example_db> INSERT INTO cyclist_name (id, lastname, firstname) VALUES (c4b65263-fe58-4846-83e8-f0e1c13d518d,'Contreras','Laura');
cqlsh:example_db> SELECT * FROM cyclist_name;

 id                                   | firstname | lastname
--------------------------------------+-----------+-----------
 c4b65263-fe58-4846-83e8-f0e1c13d518d |     Laura | Contreras
 c4b65263-fe58-4846-83e8-f0e1c13d518f |    Carlos |  Alvarado

(2 rows)
cqlsh:example_db> exit

```

