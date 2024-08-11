1) Weight: 6
Deploy a pod named nginx-pod using the nginx:alpine image.


Once done, click on the Next Question button in the top right corner of this panel. You may navigate back and forth freely between all questions. Once done with all questions, click on End Exam. Your work will be validated at the end and score shown. Good Luck!

Name: nginx-pod

Image: nginx:alpine
A) Apply below manifests:

Details
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx-pod
  name: nginx-pod
spec:
  containers:
  - image: nginx:alpine
    name: nginx-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

------------------------------------
2) Weight: 8
Deploy a messaging pod using the redis:alpine image with the labels set to tier=msg.


Pod Name: messaging

Image: redis:alpine

Labels: tier=msg

A) Run below command which create a pod with labels:

Details
> kubectl run messaging --image=redis:alpine --labels=tier=msg
------------------------------------
3) Weight: 4
Create a namespace named apx-x9984574.


Namespace: apx-x9984574

A) Run below command to create a namespace:

Details
> kubectl create namespace apx-x9984574
------------------------------------
4) Weight: 7
Get the list of nodes in JSON format and store it in a file at /opt/outputs/nodes-z3444kd9.json.


Task completed

A) Use the below command which will redirect the o/p:

Details
> kubectl get nodes -o json > /opt/outputs/nodes-z3444kd9.json

------------------------------------
5) Weight: 12
Create a service messaging-service to expose the messaging application within the cluster on port 6379.


Use imperative commands.

Service: messaging-service

Port: 6379

Type: ClusterIp

Use the right labels

A)Execute below command which will expose the pod on port 6379:

Details
> kubectl expose pod messaging --port=6379 --name messaging-service
------------------------------------
6) Weight: 11
Create a deployment named hr-web-app using the image kodekloud/webapp-color with 2 replicas.


Name: hr-web-app

Image: kodekloud/webapp-color

Replicas: 2

A)Apply below manifests:

Details
```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: hr-web-app
  name: hr-web-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hr-web-app
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: hr-web-app
    spec:
      containers:
      - image: kodekloud/webapp-color
        name: webapp-color
        resources: {}
status: {}
```
In v1.19, we can add --replicas flag with kubectl create deployment command:

> kubectl create deployment hr-web-app --image=kodekloud/webapp-color --replicas=2

------------------------------------
7) Weight: 8
Create a static pod named static-busybox on the controlplane node that uses the busybox image and the command sleep 1000.


Name: static-busybox

Image: busybox

A) To Create a static pod, copy it to the static pods directory. In this case, it is /etc/kubernetes/manifests. Apply below manifests:

Details
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: static-busybox
  name: static-busybox
spec:
  containers:
  - command:
    - sleep
    - "1000"
    image: busybox
    name: static-busybox
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

------------------------------------
8) Weight: 12
Create a POD in the finance namespace named temp-bus with the image redis:alpine.


Name: temp-bus

Image Name: redis:alpine

A)Run below command to create a pod in namespace finance:

Details
> kubectl run temp-bus --image=redis:alpine -n finance

------------------------------------
9) Weight: 8
A new application orange is deployed. There is something wrong with it. Identify and fix the issue.


Issue fixed

A)Run below command and troubleshoot step by step:

Details
> kubectl describe pod orange
Export the running pod using below command and correct the spelling of the command sleeeep to sleep

> kubectl get pod orange -o yaml > orange.yaml
Delete the running Orange pod and recreate the pod using command.

> kubectl delete pod orange
> kubectl create -f orange.yaml

------------------------------------
10) Weight: 10
Expose the hr-web-app as service hr-web-app-service application on port 30082 on the nodes on the cluster.


The web application listens on port 8080.

Name: hr-web-app-service

Type: NodePort

Endpoints: 2

Port: 8080

NodePort: 30082

A)Apply below manifests:

Details
```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: hr-web-app
  name: hr-web-app-service
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
    nodePort: 30082
  selector:
    app: hr-web-app
  type: NodePort
status:
  loadBalancer: {}
```
------------------------------------
11) Weight: 6
Use JSON PATH query to retrieve the osImages of all the nodes and store it in a file /opt/outputs/nodes_os_x43kj56.txt.


The osImages are under the nodeInfo section under status of each node.

Task Completed

A)Run the below command to redirect the o/p:

Details
> kubectl get nodes -o jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt

------------------------------------
12) Weight: 8
Create a Persistent Volume with the given specification: -

Volume name: pv-analytics

Storage: 100Mi

Access mode: ReadWriteMany

Host path: /pv/data-analytics


Is the volume name set?

Is the storage capacity set?

Is the accessMode set?

Is the hostPath set?


A)Apply the below manifest to create a PV:

Details
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-analytics
spec:
  capacity:
    storage: 100Mi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  hostPath:
      path: /pv/data-analytics
```
------------------------------------
