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
The command to outputs the pod's yaml descriptor gives a more detailed information includeding the content of the yaml file used for creating the pod. Whlie the 'kubectl describe' command only gives out important information about the pod.
### (1d) Can you access your pod with curl localhost:8080 (or other port)? If no, why not?
No. I cannot access it because the pod is not running in the localhost, it is running within the minikube cluster. To access the pod from localhost, I need to do port forwarding; using kubectl to set up a proxy that will forward all traffic from a given port in localhost to a port associated with the pod running in the minikube cluster.
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
If no namespace is specified, your command would be executed in the default namespace. But you can find out your current namespace with the command:
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
### (5a) Write a pod manifest miniwhoami-rc.yaml for a replication controller spawning 3 miniwhoami pods.
### (5b) Delete a pod. What command do you use to do this? What is happening?
`kubectl get po`
`kubectl delete po miniwhoami-rc-tqvcn`
```
NAME                  READY   STATUS        RESTARTS   AGE
miniwhoami-rc-tqvcn   1/1     Terminating   0          2m15s
miniwhoami-rc-x8bdr   1/1     Running       0          2m15s
miniwhoami-rc-xjl9c   1/1     Running       0          14s
miniwhoami-rc-zw5n9   1/1     Running       0          2m15s
```
* The replication controller creats a new pod as the pod `miniwhoami-rc-tqvcn` gets deleted. When the ReplicationController receives a notification of a pod being deleted, the controller is prompted to inspect the actual number of pods and take the necessary steps to return to the desired state of three (3) pods.

### (5c) Output and interpret the description of the replication controller.
`kubectl describe rc miniwhoami-rc`
```
Name:         miniwhoami-rc
Namespace:    default
Selector:     app=miniwhoami
Labels:       app=miniwhoami
Annotations:  <none>
Replicas:     3 current / 3 desired  #  ------> "The current number of pods is 3 and the desired number of pod instances is 3."
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed  # -----> "Pod intance per pod status"
...

# -----> "The events related to this ReplicationController"

Events:
  Type    Reason            Age    From                    Message
  ----    ------            ----   ----                    -------
  Normal  SuccessfulCreate  4m26s  replication-controller  Created pod: miniwhoami-rc-zw5n9
  Normal  SuccessfulCreate  4m26s  replication-controller  Created pod: miniwhoami-rc-x8bdr
  Normal  SuccessfulCreate  4m26s  replication-controller  Created pod: miniwhoami-rc-tqvcn
  Normal  SuccessfulCreate  2m25s  replication-controller  Created pod: miniwhoami-rc-xjl9c
  ```
### (5d) Edit your pod manifest by adding an additional label app2: new to the template . What command can you use to make the change on the fly?
`kubectl edit rc miniwhoami-rc`
### (5e) Delete a pod. What is happening? View the labels of the pods. What is the result ?
```
NAME                  READY   STATUS        RESTARTS   AGE     LABELS
miniwhoami-rc-hw69f   1/1     Running       0          6s      app2=new,app=miniwhoami
miniwhoami-rc-x8bdr   1/1     Terminating   0          11m     app=miniwhoami
miniwhoami-rc-xjl9c   1/1     Running       0          9m10s   app=miniwhoami
miniwhoami-rc-zw5n9   1/1     Running       0          11m     app=miniwhoami
```

* Before I deleted the pod, the additional label `app2: new` to the pod template did not trigger any change from the Replication controller because according to the defined selector  `app=miniwhoami`, the desired state of three (3) pods with labels `app=miniwhoami` is already fulfilled. 

* But deleting the pod means the ReplicationController sees inadequate number of pods. Hence, as the deleted pod  `miniwhoami-rc-x8bdr` is terminating the replication controller creates a new pod `miniwhoami-rc-hw69f`  using the edited pod template. Hence, the new pod has both labels `app2=new` and `app=miniwhoami`.

### (5f) Edit your pod manifest by replacing the previous selector `app: miniwhoami` with the selector `app2: new` at the replication controller. Observe and describe what happens during this process.
```
NAME                  READY   STATUS    RESTARTS   AGE   LABELS
miniwhoami-rc-hw69f   1/1     Running   0          10m   app2=new,app=miniwhoami
miniwhoami-rc-l92j5   1/1     Running   0          25s   app2=new,app=miniwhoami
miniwhoami-rc-rkbwf   1/1     Running   0          25s   app2=new,app=miniwhoami
miniwhoami-rc-xjl9c   1/1     Running   0          19m   app=miniwhoami
miniwhoami-rc-zw5n9   1/1     Running   0          21m   app=miniwhoami

```

* The ReplicationController created two new pods `miniwhoami-rc-l92j5` and `miniwhoami-rc-rkbwf` . This is in response to the new selector `app2: new` ; the controller is ensuring that the new desired state of three (3) pods with the labels `app2=new` and `app=miniwhoami` is fulfilled.

* The two pods `miniwhoami-rc-xjl9c ` and `miniwhoami-rc-zw5n9 ` that were initially created with the old pod template having only the label `app=miniwhoami` have been moved out of the scope of the ReplicationController.

## Task 6 - Replication Set
### (6a) Clean up your system completely again. Next, use the replication controller pod manifest `miniwhoami-rc.yaml` from Task 5 above to start three miniwhoami pods.
### (6b) Delete the replication controller you just created without also deleting the pods it manages. Which command do you use for this? Check the situation that has arisen, i.e check if the replication controller is deleted and the pods are still running.
`kubectl delete rc miniwhoami-rc --cascade=orphan`
* The pods were still running even after deleting the ReplicationController.
### (6c) Write a pod manifest `miniwhoami-rs.yaml` for a ReplicaSet that spawns five(!) miniwhoami pods with the label app: miniwhoami  
### (6d) Check the current running pods, then use miniwhoami-rs.yaml to create the replication set and check the running pods.
### (6e) Interpret your observations.
```
miniwhoami-rc-4m9qw   1/1     Running   0          27m     app=miniwhoami
miniwhoami-rc-mrgxn   1/1     Running   0          27m     app=miniwhoami
miniwhoami-rc-skvbl   1/1     Running   0          27m     app=miniwhoami
miniwhoami-rs-q8926   1/1     Running   0          2m34s   app=miniwhoami
miniwhoami-rs-t82sq   1/1     Running   0          2m34s   app=miniwhoami
```
* Though the desired state is five(5) pods, only the two pods `miniwhoami-rs-q8926` and `miniwhoami-rs-t82sq` were created by the ReplicaSet. The previously running three(3) pods were added to the scope of the ReplicaSet because they meet the Label - Selector requirement defined in the ReplicaSet. 

### (6f) As an experimental conclusion, start the old replication controller miniwhoami-rc.yaml from the previous task, 3 miniwhoami Pods again. What is happening? How do you interpret your observations?
* The ReplicationController created three(3) new pods even when pods with the same label `àpp:miniwhoami` in the scope of the ReplicaSet were already running.






