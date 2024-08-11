1) Weight: 12
Create a new service account with the name pvviewer. Grant this Service account access to list all PersistentVolumes in the cluster by creating an appropriate cluster role called pvviewer-role and ClusterRoleBinding called pvviewer-role-binding.
Next, create a pod called pvviewer with the image: redis and serviceAccount: pvviewer in the default namespace.


ServiceAccount: pvviewer

ClusterRole: pvviewer-role

ClusterRoleBinding: pvviewer-role-binding

Pod: pvviewer

Pod configured to use ServiceAccount pvviewer ?

A) Run below command for solution

Details

> kubectl create serviceaccount pvviewer
> kubectl create clusterrole pvviewer-role --resource=persistentvolumes --verb=list
> kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pvviewer
  name: pvviewer
spec:
  containers:
  - image: redis
    name: pvviewer
    resources: {}
  serviceAccountName: pvviewer
```
---------------------------

2) Weight: 12
List the InternalIP of all nodes of the cluster. Save the result to a file /root/CKA/node_ips.


Answer should be in the format: InternalIP of controlplane<space>InternalIP of node01 (in a single line)

Task Completed

A) Details
> kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}' > /root/CKA/node_ips

---------------------------

3) Weight: 12
Create a pod called multi-pod with two containers.
Container 1: name: alpha, image: nginx
Container 2: name: beta, image: busybox, command: sleep 4800

Environment Variables:
container 1:
name: alpha

Container 2:
name: beta


Pod Name: multi-pod

Container 1: alpha

Container 2: beta

Container beta commands set correctly?

Container 1 Environment Value Set

Container 2 Environment Value Set

A) 
Run the below command for solution:

Details
```
apiVersion: v1
kind: Pod
metadata:
  name: multi-pod
spec:
  containers:
  - image: nginx
    name: alpha
    env:
    - name: name
      value: alpha
  - image: busybox
    name: beta
    command: ["sleep", "4800"]
    env:
    - name: name
      value: beta
status: {}
```
---------------------------


4) Weight: 8
Create a Pod called non-root-pod , image: redis:alpine

runAsUser: 1000

fsGroup: 2000


Pod non-root-pod fsGroup configured

Pod non-root-pod runAsUser configured

A) 
Run the below command for solution:

Details
```
apiVersion: v1
kind: Pod
metadata:
  name: non-root-pod
spec:
  securityContext:
    runAsUser: 1000
    fsGroup: 2000
  containers:
  - name: non-root-pod
    image: redis:alpine
```
---------------------------


5) Weight: 14
We have deployed a new pod called np-test-1 and a service called np-test-service. Incoming connections to this service are not working. Troubleshoot and fix it.
Create NetworkPolicy, by the name ingress-to-nptest that allows incoming connections to the service over port 80.


Important: Don't delete any current objects deployed.

Important: Don't Alter Existing Objects!

NetworkPolicy: Applied to All sources (Incoming traffic from all pods)?

NetWorkPolicy: Correct Port?

NetWorkPolicy: Applied to correct Pod?

A) Run the below command for solution:

Details
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ingress-to-nptest
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: np-test-1
  policyTypes:
  - Ingress
  ingress:
  - ports:
    - protocol: TCP
      port: 80
```
---------------------------


6) Weight: 12
Taint the worker node node01 to be Unschedulable. Once done, create a pod called dev-redis, image redis:alpine, to ensure workloads are not scheduled to this worker node. Finally, create a new pod called prod-redis and image: redis:alpine with toleration to be scheduled on node01.


key: env_type, value: production, operator: Equal and effect: NoSchedule

Key = env_type

Value = production

Effect = NoSchedule

pod 'dev-redis' (no tolerations) is not scheduled on node01?

Create a pod 'prod-redis' to run on node01

A) Run the below command for solution:

Details
> kubectl taint node node01 env_type=production:NoSchedule
Deploy dev-redis pod and to ensure that workloads are not scheduled to this node01 worker node.

> kubectl run dev-redis --image=redis:alpine

> kubectl get pods -owide
Deploy new pod prod-redis with toleration to be scheduled on node01 worker node.
```
apiVersion: v1
kind: Pod
metadata:
  name: prod-redis
spec:
  containers:
  - name: prod-redis
    image: redis:alpine
  tolerations:
  - effect: NoSchedule
    key: env_type
    operator: Equal
    value: production     
```
View the pods with short details:

> kubectl get pods -owide | grep prod-redis
---------------------------


7) Weight: 8
Create a pod called hr-pod in hr namespace belonging to the production environment and frontend tier .
image: redis:alpine


Use appropriate labels and create all the required objects if it does not exist in the system already.

hr-pod labeled with environment production?

hr-pod labeled with tier frontend?

A) Run the below command for solution:

Details
> kubectl create namespace hr
> kubectl run hr-pod --image=redis:alpine --namespace=hr --labels=environment=production,tier=frontend

---------------------------


8) Weight: 8
A kubeconfig file called super.kubeconfig has been created under /root/CKA. There is something wrong with the configuration. Troubleshoot and fix it.


Fix /root/CKA/super.kubeconfig

A) Run the below command for solution:

Details
```
vi /root/CKA/super.kubeconfig

Change the 2379 port to 6443 and run the below command to verify

kubectl cluster-info --kubeconfig=/root/CKA/super.kubeconfig
```
---------------------------


9) Weight: 14
We have created a new deployment called nginx-deploy. scale the deployment to 3 replicas. Has the replica's increased? Troubleshoot the issue and fix it.


deployment has 3 replicas

A) Run the below command for solution:

Details
> sed -i 's/kube-contro1ler-manager/kube-controller-manager/g' kube-controller-manager.yaml

---------------------------

