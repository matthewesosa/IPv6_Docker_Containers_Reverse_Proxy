# Internship Sheet 7 - Minikube

# Task 1 - Installing Minikube
a) Install the latest version of Minikube (at least v1.25.2) and the Kubernetes CLI (at least v1.23.6) on your workstation.
Briefly describe your installation steps.
b) Verify the installed versions with minikube version and kubectl version.
c) Test your installation with the command sequence minikube start / minikube status / kubectl get nodes / minikube stop .
d) As a test, launch the web-based Kubernetes dashboard. What command do you use to do this?
e) Describe the system architecture of the minikube environment you installed.

# Task 2 - Set up and test Minikube
In this and the following tasks, which refer to the literature basis Kubernetes in Action , please use kubia your container miniwhoamithat you have uploaded to your registry on your Gitlab account instead of the container. If this is not available to you or if you cannot access it, you can use the container jennerwein/miniwhoami instead.

a) Set up an alias k and tab completion for the kubectl command.
b) Check the functionality of your cluster (and alias k) using the short commands k get nodes and k cluster-info .
c) Retrieve more information about your node minikube using the kubectl describe nodes minikube command. What information does the command give you?
d) You can use the kubectl run command (https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run) to start a container as a pod in a very primitive way. This primitive command is not typical for Kubernetes and should only be used here for testing.

i) Start - just for testing - a pod miniwhoami on your minikube. Specify the command you are using.
ii) Use the kubectl get pods command and the Kubernetes dashboard to check if the pod is running.
iii) Retrieve information about the pod using the kubectl describe pods miniwhoami command. Briefly, describe the information obtained.
iv) Delete the pod. What command did you use to do this?

# Task 3
To prepare for next week's presentations, read from Lukša Marko: [Kubernetes in Action](https://www.manning.com/books/kubernetes-in-action), Hanser, 2018:

* Chapter 6 - Volumes: attaching disk storage to containers.
* Chapter 7 - ConfigMaps and Secrets: configuring application. managed pods.
* Chapter 8 - Accessing pod metadata and other resources from applications. to pods.
* Chapter 9 - Deployments: updating applications declaratively. to pods.
  
Test your knowledge with the following questionnaire: [questions2-k8s.html](https://leischner.inf.h-brs.de/lehre/servmgmt/questions2-k8s.html). (Answers do not need to be documented in GIT).