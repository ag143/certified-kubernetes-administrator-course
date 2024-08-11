1) Weight: 15
Upgrade the current version of kubernetes from 1.29.0 to 1.30.0 exactly using the kubeadm utility. Make sure that the upgrade is carried out one node at a time starting with the controlplane node. To minimize downtime, the deployment gold-nginx should be rescheduled on an alternate node before upgrading each node.


Upgrade controlplane node first and drain node node01 before upgrading it. Pods for gold-nginx should run on the controlplane node subsequently.

Cluster Upgraded?

pods 'gold-nginx' running on controlplane?

A) 
Upgrade the current version of kubernetes from 1.28.0 to 1.29.0 exactly using the kubeadm utility.
Make sure that the upgrade is carried out one node at a time starting with the controlplane node. To minimize downtime, the deployment gold-nginx should be rescheduled on an alternate node before upgrading each node.

Upgrade controlplane node first and drain node node01 before upgrading it. Pods for gold-nginx should run on the controlplane node subsequently.

### Upgrade controlplane

* Update package repo

```
apt update
```
* Check madison to see what kubernetes packages are available

```
apt-cache madison kubeadm
```
Note that only 1.28 versions are present, meaning you have to grab the 1.29 repos

* Grab kubernetes 1.29 repos

For this, we need to edit the apt source file which you should find is /etc/apt/sources.list.d/kubernetes.list
```
vi /etc/apt/sources.list.d/kubernetes.list
```
FInd any occurrence of 1.28 in this file and change it to 1.29, then save and exit from vi.

* Now run madison again to find out the package version for 1.29
```
apt-cache madison kubeadm
```
You should see the following in the list

> kubeadm | 1.29.0-1.1 | https://pkgs.k8s.io/core:/stable:/v1.29/deb  Packages

Now we know the package version is 1.29.0-1.1 we can proceed with the upgrade

* Drain node

> kubectl drain controlplane --ignore-daemonsets
* Upgrade kubeadm

```
apt-mark unhold kubeadm
apt install -y kubeadm=1.29.0-1.1
```
* Plan and apply upgrade
```
kubeadm upgrade plan
kubeadm upgrade apply v1.29.0
```
* Upgrade the kubelet

```
apt-mark unhold kubelet
apt install -y kubelet=1.29.0-1.1
systemctl daemon-reload
systemctl restart kubelet
```
* Reinstate controlplane node

> kubectl uncordon controlplane
* Upgrade kubectl
```
apt-mark unhold kubectl
apt install -y kubectl=1.29.0-1.1
```
* Re-hold packages

> apt-mark hold kubeadm kubelet kubectl
### Upgrade node01

* Drain the worker node

> kubectl drain node01 --ignore-daemonsets
* Go to worker node

> ssh node01
* As before, you will need to update the package caches for v1.29

Follow the same steps as you did on controlplane

* Upgrade kubeadm
```
apt-mark unhold kubeadm
apt install -y kubeadm=1.29.0-1.1
```

* Upgrade node

> kubeadm upgrade node

* Upgrade the kubelet
```
apt-mark unhold kubelet
apt install kubelet=1.29.0-1.1
systemctl daemon-reload
systemctl restart kubelet
```

> Re-hold packages

> apt-mark hold kubeadm kubelet
* Return to controlplane

> exit
* Reinstate worker node

> kubectl uncordon node01
* Verify gold-nginx is scheduled on controlplane node

> kubectl get pods -o wide | grep gold-nginx
TIP

To do cluster upgrades faster and save at least 3 minutes, you can work on both nodes at the same time.

While kubeadm upgrade apply is running on controlplane, which takes some minutes, open a second terminal and perform steps ii, iii and iv of "Upgrade node01", so that it is ready for kubeadm upgrade node as soon as you have drained it
-----------------


2) Weight: 15
Print the names of all deployments in the admin2406 namespace in the following format:

DEPLOYMENT   CONTAINER_IMAGE   READY_REPLICAS   NAMESPACE

<deployment name>   <container image used>   <ready replica count>   <Namespace>
. The data should be sorted by the increasing order of the deployment name.


Example:

DEPLOYMENT   CONTAINER_IMAGE   READY_REPLICAS   NAMESPACE
deploy0   nginx:alpine   1   admin2406
Write the result to the file /opt/admin2406_data.


Task completed?

A) Print the names of all deployments in the admin2406 namespace in the following format...
This is a job for custom-columns output of kubectl

> kubectl -n admin2406 get deployment -o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY_REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace --sort-by=.metadata.name > /opt/admin2406_data


-------------------------
3) Weight: 8
A kubeconfig file called admin.kubeconfig has been created in /root/CKA. There is something wrong with the configuration. Troubleshoot and fix it.


Fix /root/CKA/admin.kubeconfig
A)A kubeconfig file called admin.kubeconfig has been created in /root/CKA. There is something wrong with the configuration. Troubleshoot and fix it.
First, let's test this kubeconfig

> kubectl get pods --kubeconfig /root/CKA/admin.kubeconfig
Notice the error message.

Now look at the default kubeconfig for the correct setting.

> cat ~/.kube/config
Make the correction

> vi /root/CKA/admin.kubeconfig
Test

> kubectl get pods --kubeconfig /root/CKA/admin.kubeconfig


-------------------------
4) Weight: 12
Create a new deployment called nginx-deploy, with image nginx:1.16 and 1 replica. Next upgrade the deployment to version 1.17 using rolling update.


Image: nginx:1.16

Task: Upgrade the version of the deployment to 1:17
A)Create a new deployment called nginx-deploy, with image nginx:1.16 and 1 replica. Next upgrade the deployment to version 1.17 using rolling update.
```
kubectl create deployment nginx-deploy --image=nginx:1.16
kubectl set image deployment/nginx-deploy nginx=nginx:1.17 --record
```
You may ignore the deprecation warning.


-------------------------

5) Weight: 20
A new deployment called alpha-mysql has been deployed in the alpha namespace. However, the pods are not running. Troubleshoot and fix the issue. The deployment should make use of the persistent volume alpha-pv to be mounted at /var/lib/mysql and should use the environment variable MYSQL_ALLOW_EMPTY_PASSWORD=1 to make use of an empty root password.


Important: Do not alter the persistent volume.

Troubleshoot and fix the issues
A)A new deployment called alpha-mysql has been deployed in the alpha namespace. However, the pods are not running. Troubleshoot and fix the issue.
The deployment should make use of the persistent volume alpha-pv to be mounted at /var/lib/mysql and should use the environment variable MYSQL_ALLOW_EMPTY_PASSWORD=1 to make use of an empty root password.

Important: Do not alter the persistent volume.

Inspect the deployment to check the environment variable is set. Here I'm using yq which is like jq but for YAML to not have to view the entire deployment YAML, just the section beneath containers in the deployment spec.

> kubectl get deployment -n alpha alpha-mysql  -o yaml | yq e .spec.template.spec.containers -
Find out why the deployment does not have minimum availability. We'll have to find out the name of the deployment's pod first, then describe the pod to see the error.
```
kubectl get pods -n alpha
kubectl describe pod -n alpha alpha-mysql-xxxxxxxx-xxxxx
```
We find that the requested PVC isn't present, so create it. First, examine the Persistent Volume to find the values for access modes, capacity (storage), and storage class name

> kubectl get pv alpha-pv
Now use vi to create a PVC manifest
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-alpha-pvc
  namespace: alpha
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: slow
```
-------------------------

6)Weight: 10
Take the backup of ETCD at the location /opt/etcd-backup.db on the controlplane node.


Troubleshoot and fix the issues
A)Take the backup of ETCD at the location /opt/etcd-backup.db on the controlplane node.
This question is a bit poorly worded. It requires us to make a backup of etcd and store the backup at the given location.

Know that the certificates we need for authentication of etcdctl are located in /etc/kubernetes/pki/etcd
```
ETCDCTL_API='3' etcdctl snapshot save \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  /opt/etcd-backup.db
```
Whilst we could also use the argument --endpoints=127.0.0.1:2379, it is not necessary here as we are on the controlplane node, same as etcd itself. The default endpoint is the local host.


-------------------------
7) Weight: 20
Create a pod called secret-1401 in the admin1401 namespace using the busybox image. The container within the pod should be called secret-admin and should sleep for 4800 seconds.

The container should mount a read-only secret volume called secret-volume at the path /etc/secret-volume. The secret being mounted has already been created for you and is called dotfile-secret.


Pod created correctly?

A) Create a pod called secret-1401 in the admin1401 namespace using the busybox image....
The container within the pod should be called secret-admin and should sleep for 4800 seconds.

The container should mount a read-only secret volume called secret-volume at the path /etc/secret-volume. The secret being mounted has already been created for you and is called dotfile-secret.

Use imperative command to get a starter manifest

> kubectl run secret-1401 -n admin1401 --image busybox --dry-run=client -o yaml --command -- sleep 4800 > admin.yaml
Edit this manifest to add in the details for mounting the secret

> vi admin.yaml
Add in the volume and volume mount sections seen below
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: secret-1401
  name: secret-1401
  namespace: admin1401
spec:
  volumes:
  - name: secret-volume
    secret:
      secretName: dotfile-secret
  containers:
  - command:
    - sleep
    - "4800"
    image: busybox
    name: secret-admin
    volumeMounts:
    - name: secret-volume
      readOnly: true
      mountPath: /etc/secret-volume
```


And create the pod

> kubectl create -f admin.yaml






