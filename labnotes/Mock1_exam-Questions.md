1) Weight: 6
Deploy a pod named nginx-pod using the nginx:alpine image.


Once done, click on the Next Question button in the top right corner of this panel. You may navigate back and forth freely between all questions. Once done with all questions, click on End Exam. Your work will be validated at the end and score shown. Good Luck!

Name: nginx-pod

Image: nginx:alpine

2) Weight: 8
Deploy a messaging pod using the redis:alpine image with the labels set to tier=msg.


Pod Name: messaging

Image: redis:alpine

Labels: tier=msg

3) Weight: 4
Create a namespace named apx-x9984574.


Namespace: apx-x9984574

4) Weight: 7
Get the list of nodes in JSON format and store it in a file at /opt/outputs/nodes-z3444kd9.json.


Task completed

5) Weight: 12
Create a service messaging-service to expose the messaging application within the cluster on port 6379.


Use imperative commands.

Service: messaging-service

Port: 6379

Type: ClusterIp

Use the right labels

6) Weight: 11
Create a deployment named hr-web-app using the image kodekloud/webapp-color with 2 replicas.


Name: hr-web-app

Image: kodekloud/webapp-color

Replicas: 2

7) Weight: 8
Create a static pod named static-busybox on the controlplane node that uses the busybox image and the command sleep 1000.


Name: static-busybox

Image: busybox

8) Weight: 12
Create a POD in the finance namespace named temp-bus with the image redis:alpine.


Name: temp-bus

Image Name: redis:alpine

9) Weight: 8
A new application orange is deployed. There is something wrong with it. Identify and fix the issue.


Issue fixed

10) Weight: 10
Expose the hr-web-app as service hr-web-app-service application on port 30082 on the nodes on the cluster.


The web application listens on port 8080.

Name: hr-web-app-service

Type: NodePort

Endpoints: 2

Port: 8080

NodePort: 30082

11) Weight: 6
Use JSON PATH query to retrieve the osImages of all the nodes and store it in a file /opt/outputs/nodes_os_x43kj56.txt.


The osImages are under the nodeInfo section under status of each node.

Task Completed

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
