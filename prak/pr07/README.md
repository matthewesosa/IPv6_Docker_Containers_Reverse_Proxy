# Internship Sheet 7 - Minikube

## Task 1 - Installing Minikube
### (1a.) Install the latest version of Minikube (at least v1.25.2) and the Kubernetes CLI (at least v1.23.6) on your workstation. Briefly describe your installation steps.

* Minikube Setup on Linux: these instructions are valid for Debian, Ubuntu, or Mint Linux distributions.
#

* Install Minikube:

`curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`

`sudo install minikube-linux-amd64 /usr/local/bin/minikube`

* Starting Minikube and Testing Installation
  
After installing Minikube we need to start and test the cluster to make sure everything is working correctly.
(i) Add your user to the docker group
If this step was already performed when Docker was installed, it can be skipped:

`sudo usermod -aG docker $USER && newgrp docker`

Log out of the user profile and log back in so these changes take effect. If running inside a VM, you will need to restart the entire machine, not just log out.


(ii) Start with the default driver:
minikube start

(iii) Check Minikube Status
After you see a 'Done' message in your terminal, run `minikube status` to make sure the cluster is healthy. Pay particular attention that the apiserver is in a "Running" state.

(iv) Install kubectl

`curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"`

`sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl`

(v) Test kubectl
Lastly, open up your terminal and make sure that you can run `kubectl version`


### (1d.) To test, launch the web-based Kubernetes dashboard. What command do you use to do this?
  `minikube start`
  `minikube dashboard`

### (1e.) Describe the system architecture of the Minikube environment you installed.
It is a single node cluster contained in a virtual machine (VM). This cluster allows the demo Kubernetes operations without requiring the time and resource-consuming installation of full-blown K8s.

## Task 2 - Set up and test Minikube

### In this and the following tasks related to the Kubernetes in Action literature base, instead of using the kubia container, please use your miniwhoami container that you uploaded to your registry on your Gitlab account. If this is not available to you or if you do not succeed in accessing it, you can also use the container jennerwein/miniwhoami as a substitute.
#
### (2c) Get more information about your node minikube using the command kubectl describe nodes minikube. What information does the command give you?
`kubectl describe nodes minikube`

```

Name:               minikube
Roles:              control-plane
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=minikube
                    kubernetes.io/os=linux
                    minikube.k8s.io/commit=986b1ebd987211ed16f8cc10aed7d2c42fc8392f
                    minikube.k8s.io/name=minikube
                    minikube.k8s.io/primary=true
                    minikube.k8s.io/updated_at=2022_11_09T17_00_11_0700
                    minikube.k8s.io/version=v1.28.0
                    node-role.kubernetes.io/control-plane=
                    node.kubernetes.io/exclude-from-external-load-balancers=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: unix:///var/run/cri-dockerd.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Wed, 09 Nov 2022 17:00:07 +0100
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  minikube
  AcquireTime:     <unset>
  RenewTime:       Mon, 28 Nov 2022 10:15:13 +0100
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Mon, 28 Nov 2022 10:12:49 +0100   Wed, 09 Nov 2022 16:59:59 +0100   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Mon, 28 Nov 2022 10:12:49 +0100   Wed, 09 Nov 2022 16:59:59 +0100   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Mon, 28 Nov 2022 10:12:49 +0100   Wed, 09 Nov 2022 16:59:59 +0100   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Mon, 28 Nov 2022 10:12:49 +0100   Wed, 09 Nov 2022 17:00:22 +0100   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  192.168.49.2
  Hostname:    minikube
Capacity:
  cpu:                4
  ephemeral-storage:  21777232Ki
  hugepages-2Mi:      0
  memory:             8140376Ki
  pods:               110
Allocatable:
  cpu:                4
  ephemeral-storage:  21777232Ki
  hugepages-2Mi:      0
  memory:             8140376Ki
  pods:               110
System Info:
  Machine ID:                 996614ec4c814b87b7ec8ebee3d0e8c9
  System UUID:                4f5bcf43-0644-4151-be6e-db8ba64dbe70
  Boot ID:                    e1f64b67-b3af-4107-84a0-11f37d903621
  Kernel Version:             5.15.0-53-generic
  OS Image:                   Ubuntu 20.04.5 LTS
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  docker://20.10.20
  Kubelet Version:            v1.25.3
  Kube-Proxy Version:         v1.25.3
PodCIDR:                      10.244.0.0/24
PodCIDRs:                     10.244.0.0/24
Non-terminated Pods:          (10 in total)
  Namespace                   Name                                         CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                         ------------  ----------  ---------------  -------------  ---
  ingress-nginx               ingress-nginx-controller-5959f988fd-8lksw    100m (2%)     0 (0%)      90Mi (1%)        0 (0%)         6d17h
  kube-system                 coredns-565d847f94-8444q                     100m (2%)     0 (0%)      70Mi (0%)        170Mi (2%)     18d
  kube-system                 etcd-minikube                                100m (2%)     0 (0%)      100Mi (1%)       0 (0%)         18d
  kube-system                 kube-apiserver-minikube                      250m (6%)     0 (0%)      0 (0%)           0 (0%)         18d
  kube-system                 kube-controller-manager-minikube             200m (5%)     0 (0%)      0 (0%)           0 (0%)         18d
  kube-system                 kube-proxy-hbt6v                             0 (0%)        0 (0%)      0 (0%)           0 (0%)         18d
  kube-system                 kube-scheduler-minikube                      100m (2%)     0 (0%)      0 (0%)           0 (0%)         18d
  kube-system                 storage-provisioner                          0 (0%)        0 (0%)      0 (0%)           0 (0%)         18d
  kubernetes-dashboard        dashboard-metrics-scraper-b74747df5-wrq8p    0 (0%)        0 (0%)      0 (0%)           0 (0%)         70m
  kubernetes-dashboard        kubernetes-dashboard-57bbdc5f89-rf2bg        0 (0%)        0 (0%)      0 (0%)           0 (0%)         70m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                850m (21%)  0 (0%)
  memory             260Mi (3%)  170Mi (2%)
  ephemeral-storage  0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
Events:              <none>

```

### (2d.i) Just for testing, start a pod miniwhoami on your minikube. Enter the command you used.

`kubectl run miniwhoami --image=docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami`

### 2d.iii) Retrieve information about the pod using the kubectl describe pods miniwhoami command. Briefly, describe the information obtained.
* The information displayed when the command `kubectl describe pods miniwhoami` is used includes the pod name, IP, start time, status e.t.c

### (2d.iv) Delete the pod. What command did you use to do this?

`kubectl delete pod miniwhoami`

