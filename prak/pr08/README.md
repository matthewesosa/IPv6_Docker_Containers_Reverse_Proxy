# Internship Sheet 8 - Pods

## Task 1 - Podmanifest
### Start your pod miniwhoami-manual. 
### (1b.i)What command did you use to start your pod? 
`kubectl apply -f miniwhoami-manual.yml`
### (1b.ii) What command can you use to get the logs of the started pod miniwhoami-manual?
`kubectl logs miniwhoami-manual`
### (1c) Output your pod's YAML descriptor. 
### (ic.i) What is the qosClass of your pod? 
* qosClass: BestEffort
### (1c.ii) What command did you use for this?
`kubectl get po miniwhoami-manual -o yaml`
### (1c.iii) What is the difference compared to the command kubectl describe ... .
The command to outputs the pod's yaml descriptor gives a more detailed information includeding the content of the yaml file used for creating the pod. Whlie the 'kbectl describe' command only gives out important information about the pod.
### (1d) Can you access your pod with curl localhost:8080 (or other port)? If no, why not?
No. I cannot access it because the pod is not running in my local machnine, it is running within the minikube virtual machine that was created in my local machine when I installed minikube.
### (1e) Provide a method to access your pod and try it. Which commands did you use for this?
I can either use a simple NodePort service to 'minikube ip' or port forwarding to my local machine.

`kubectl port-forward miniwhoami-manual 8080:80`

## Task 2 - labels
### (2b) View all running pods with the labels.
`kubectl get pods --show-labels`
```
NAME                     READY   STATUS    RESTARTS   AGE     LABELS
miniwhoami-manual        1/1     Running   0          146m    <none>
miniwhoami-manual-pod2   1/1     Running   0          2m33s   hochschule=hbrs,sem=ws22
```
### (2c) Create a pod miniwhoami-manual-pod3 with the labels hochschule: th-koeln and sem: ss19 and start it.
`kubectl apply -f miniwhoami-manual-pod3.yml`
### (2d) View all running pods with the labels.
`kubectl get po --show-labels`
```
NAME                     READY   STATUS    RESTARTS   AGE    LABELS
miniwhoami-manual        1/1     Running   0          157m   <none>
miniwhoami-manual-pod2   1/1     Running   0          13m    hochschule=hbrs,sem=ws22
miniwhoami-manual-pod3   1/1     Running   0          24s    hochschule=th-koeln,sem=ss19
```
### (2e) From Pod miniwhoami-manual-p3 change the value of Label semto ws22.
`kubectl label po miniwhoami-manual-p3 sem=ws22 --overwrite`
### (2f) Display the pods with the labels again.
` kubectl get po --show-labels`
```
NAME                     READY   STATUS    RESTARTS   AGE    LABELS
miniwhoami-manual        1/1     Running   0          170m   <none>
miniwhoami-manual-pod2   1/1     Running   0          26m    hochschule=hbrs,sem=ws22
miniwhoami-manual-pod3   1/1     Running   0          13m    hochschule=th-koeln,sem=ws22
```
## Task 3 - Namespaces
### (3a) Create a namespace using your HBRS username migbin2s as the name .
`kubectl create ns migbin2s`
### (3b) What namespaces exist in your minikube? How do you find out?
`kubectl get ns`
```
NAME                   STATUS   AGE
default                Active   23d
ingress-nginx          Active   11d
kube-node-lease        Active   23d
kube-public            Active   23d
kube-system            Active   23d
kubernetes-dashboard   Active   5d6h
migbin2s               Active   67s
```
### (3c) How do you find out your current namespace?
`kubectl config get-contexts`
```
CURRENT   NAME       CLUSTER    AUTHINFO   NAMESPACE
*         minikube   minikube   minikube   default
```
### (3d) Run the pod  miniwhoami-manual in the newly created namespace migbin2s.
`kubectl apply -f miniwhoami-manual.yml -n migbin2s`
### (3e) Check if the pod xxxxxx2sis running in the new namespace? What command do you use to do this?
`kubectl get po -n migbin2s`
### (3f) Clean up your system completely again. Which command is the easiest to use?
`kubectl delete all --all`

## Task 4 - HTTP Activity Probe
### (4d) Start the faulty pod miniwhoami . Which command do you use for this? Check if the pod is running and for how long. After how many seconds does the first problem appear? How can you tell if the pod has crashed?
`kubectl apply -f miniwhoami-liveness-probe.yml`
* I set an initial delay of 20s for the httpGet probe. And the pod ran for 2m10s then it restarted.
* The number of RESTARTS for the pod is equal to the number of times the pod has crashed.
### (4e) For this subtask only, comment out the HTTP activity probe in the pod manifest miniwhoami-liveness-probe.yaml and start the pod again. Test the pod's behavior. What behavior do you now observe and how do you interpret the behavior?  Now activate the activity probe again!
* The pod is running smoothly without restarting
* The pod is currently not receiving any request. And there is no liveness probe like an httpGet probe to monitor what could happen if several requests are sent to the running pod.
### (4f) What is the best way to observe what is happening during monitoring? Which command do you use for this?
*The best approach is to first figure out  why the previous container terminated by checking the logs using the command below:

`kubectl logs miniwhoami --previous`

* And you can see why the container had to be restarted by looking at what kubectl describe prints out:
* 
`kubectl describe po miniwhoami`
```
Name:             miniwhoami
Namespace:        default

...
 State:          Running
      Started:      Sun, 04 Dec 2022 17:29:29 +0100
    Last State:     Terminated
      Reason:       Error
      Exit Code:    137
      Started:      Sun, 04 Dec 2022 17:27:20 +0100
      Finished:     Sun, 04 Dec 2022 17:29:28 +0100
    Ready:          True
    Restart Count:  1
    Liveness:       http-get http://:8080/ delay=20s timeout=1s period=10s #success=1 #failure=3

    ...
    Events:
  Type     Reason     Age                  From               Message
  ----     ------     ----                 ----               -------
  Normal   Killing    27m (x3 over 31m)    kubelet            Container miniwhoami failed liveness probe, will be restarted
  ...
  
  ```
* The container is currently running, but because of an error, it previously terminated with an exit code of 137 (meaning it was terminated with an external  SIGKILL signal)

## Task 5 - Replication Controller
## (5b) Delete a pod. What command do you use to do this? What is happening?













