# Internship Sheet 8 - Pods

## Task 1 - Pod Manifest
a) For your miniwhoami container from Practicum Sheet 2 (Task 3), create a basic pod manifest miniwhoami-manual.yaml that describes a pod miniwhoami-manual.

b) Start your pod miniwhoami-manual. What command did you use to start your pod? Which command can you use to retrieve the log log of the started pod miniwhoami-manual?

c) Output the YAML descriptor of your pod. What is the qosClass of your pod? What command did you use for this?

What is the difference with the command kubectl describe ... .

d) Can you access your pod with curl localhost:8080 (or other port)? If no, why not?

e) Give a method for accessing your pod and try it out. What commands did you use to do this?
#
## Task 2 - Labels
a) Create a pod miniwhoami-manual-pod2 with the labels university: hbrs and sem: ws22 and start it.

b) Display all running pods with the labels.

c) Create a pod miniwhoami-manual-pod3 with the labels university: th-koeln and sem: ss19 and start it.

d) Display all running pods with the labels.
From Pod miniwhoami-manual-p3, change the value of the label sem to ws22.

Display the pods with the labels again.
#
## Task 3 - Namespaces
a) Create a namespace for which you take your HBRS username xxxxxx2s as name.

b) What namespaces exist in your minikube? How do you find them out?

c) How do you find out your current namespace?

d) Run the pod miniwhoami-manual in the newly created namespace migbin2s2s.

e) Verify that the pod is running in the new namespace xxxxxx2s? What command do you use to do this?

f) Clean up your system completely again. Which command makes it easy to do this?
#
## Task 4 - HTTP activity probe
a) Change your program miniwhoami to a program miniwhoami-crash, which crashes after the fifth program call and returns an appropriate error message.

b) First test your program locally and then push an appropriate Docker image to your registry. (If you fail to do this, you can also use the jennerwein/miniwhoami-crash image I provided in the tasks below. This image uses port 8080.)

c) Write a pod manifest miniwhoami-liveness-probe.yaml that uses an HTTP activity probe to monitor a faulty container miniwhoami. This now faulty container miniwhoami was created from the image miniwhoami-crash from your registry (or from my image jennerwein/miniwhoami-crash).

d) nStart the just defined faulty pod miniwhoami. Which command do you use for this? Check if and how long the pod runs. After how many seconds is there the first problem? How can you tell that the pod has crashed?

e) Comment out - for this subtask only - the HTTP activity probe in the pod manifest miniwhoami-liveness-probe.yaml and restart the pod. Test the behavior of the pod. What behavior do you observe now and how do you interpret the behavior?
Now enable the activity probe again!

f) What is the best way to observe what is happening during monitoring? Which command do you use for this?
#
## Task 5 - Replication Controller
a) Write a pod manifest miniwhoami-rc.yaml for a replication controller that creates 3 miniwhoami pods.

b) Delete one pod. What command do you use to do this? What happens?

c) Print the description of the replication controller and interpret it.

d) Edit your pod manifest by adding an additional label app2: new in the template. What command can you use to make the change on the fly?

e) Delete a pod. What happens. Display the tags of the pods. What is the result ?

f) Edit your pod manifest by replacing the previous selector app: miniwhoami with the selector app2: new at the replication controller. Observe and describe what happens when you do this.
#
## Task 6 - Replication Set
a) Clean up your system completely again. Then, using the replication controller pod manifest miniwhoami-rc.yaml from the previous Task 5, start three miniwhoami pods.

b) Delete the replication controller you just created without also deleting the pods it manages. What command do you use to do this? Check the created situation, i.e. check if the replication controller is deleted and the pods are still running.

c) Write a pod manifest miniwhoami-rs.yaml for a replication set that creates five(!) miniwhoami pods tagged app: miniwhoami.

d) Check the current running pods, then use miniwhoami-rs.yaml to create the replication set and check the running pods.

e) Interpret your observations.

f) For the experimental conclusion, run again the old replication controller miniwhoami-rc.yaml from the previous task 3 miniwhoami pods. What happens? How do you interpret your observations?