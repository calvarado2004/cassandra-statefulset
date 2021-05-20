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

Also you can upload some mock data to this table.

```
kubectl exec -it pod/cassandra-0 -n cassandra -- cqlsh < cyclists.cql
Unable to use a TTY - input is not a terminal or the right kind of file

$ kubectl exec -it pod/cassandra-0 -n cassandra -- cqlsh
cqlsh> SELECT * FROM example_db.cyclist_name;

 id                                   | firstname  | lastname
--------------------------------------+------------+-----------------
 dcbcb52e-a650-429b-b315-ecee5b4871b6 |       Adan |        Willmott
 d570fc54-2fba-413c-9502-58d20032e69d |    Electra |     Stockbridge
 055de49b-cb76-41e4-b4be-9a875cffdba2 |     Cletus |          Atchly
 d71d9c94-4e05-4084-a940-552577f8b9ca |       Alli |           Hedge
 050f1069-49e1-47b9-a5f3-06d122d1413e |       Poul |         Bromige
 1235bf0e-1ba2-49bc-a365-6e54da2f82ba |      Price |         Kinchin
 846abb8d-4a5b-491c-9694-21e5fddabb4a |       Shep |           Blyde
 95596356-81f9-4840-8748-c71ebad6a0e1 |   Alphonso |          Kassel
 18f12929-5eae-4c9c-829b-b2e56a1f8294 |      Dorri |            Duer
```
