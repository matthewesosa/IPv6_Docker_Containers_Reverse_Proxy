# Internship Sheet 9 - Serverless Computing
## Task 1 - Definition of serverless computing
### (1a) How can serverless computing be defined
* Serverless computing is a method to execute code in a cloud environment without caring about the underlying infrastructure. You deploy your application while  the cloud provider takes care of the execution in response to  particular 'triggers' or 'events'. This eliminates the inconvenience of managing servers.

### (1b) and how can it be distinguished from a kubernetes based PaaS offering?
* One key difference between serverless computing and a Kubernetes-based PaaS is that serverless computing is typically more abstracted from the underlying infrastructure and is designed to be triggered by specific events or actions, while a Kubernetes-based PaaS provides a more general-purpose platform for running applications in containers.
 
* Unlike other Kubernetes based PaaS, Serverless computing approach like Knative solves an important technical problem of  being able to scale resources down to zero, rather than requiring at least a minimal number to sit idle, waiting for workloads.
 

## Task 2 - Install knative on your kubernetes cluster
### Recommendation: Use minikube or kind to be able to use kn's quickstart plugin

###### Install the Knative quickstart plugin
* Find link of latest release and download 'kn-quickstart-linux-amd64' . Go to https://github.com/knative-sandbox/kn-plugin-quickstart/releases/tag/knative-v1.8.1 and find link to latest release for amd64

* Make it executable by running the command:
  
    `chmode +x kn-quickstart-linux-amd64`

* Move the executable binary file to a directory on your PATH by running the command:
  
    `mv kn-quickstart-linux-amd64 /usr/local/bin`

* Rename the executable binary to 'kn-quickstart' by running the command:
  
    `mv kn-quickstart-linux-amd64 kn-quickstart`

* Verify that the plugin is working by running the command:
  
    `kn-quickstart --help`


### a) Install knative on your kubernetes cluster.
* Run the Knative quickstart plugin:

    `kn-quickstart minikube`

* The output of the previous command asked you to run minikube tunnel. Run the following command to start the process in a second terminal window, then return to the primary window and press enter to continue:

    `minikube tunnel --profile knative`

* NOTE: The tunnel must continue to run in a terminal window any time you are using your Knative quickstart environment.
The tunnel command is required because it allows your cluster to access Knative ingress service as a LoadBalancer from your host computer.

* After the plugin is finished, verify you have a cluster called 'knative' using:

    `minikube profile list`
    

[Reference source: knative.dev](https://knative.dev/docs/install/quickstart-install/#install-the-knative-quickstart-plugin) 

### b) Create a knative service with a webapp of your choice and request it by using an http-client.
```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: webapp
spec:
  template:
    spec:
      containers:
      - image: docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami
        ports:
        - containerPort: 80
```
`kubectl apply -f webapp-kservice.yml`

```
NAME     URL                                             LATESTCREATED   LATESTREADY    READY   REASON
webapp   http://webapp.default.10.107.226.239.sslip.io   webapp-00001    webapp-00001   True
```
`http "http://webapp.default.10.107.226.239.sslip.io"`

## Task 3 - Cold-start-delay
### (3a) Describe the cases in which a cold-start-delay can occur.
* First-time function invocations: A function will often incur a cold-start delay when it is called for the first time since its container needs to be spun up and initialized.
* A function may have a cold-start delay when it is subsequently invoked if it has not been used recently. This is due to the possibility that the function's container was turned off in order to save resources.
* If you have not used a pod for a long time in a Kubernetes node or when you are creating a new pod from a new image. In both cases, it would therefore be necessary to pull the images from a remote image repository. And the container need to be restarted from scratch. Such cold-start-delay in serverless computing usually takes between 10seconds to 15seconds (and sometimes up to a minute or more).
### Create a service in knative that uses the standard knative pod autoscaler and for which the occurrence of cold-start-delays is impossible.

