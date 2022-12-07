# Internship Sheet 9 - Serverless Computing

## Task 1 - Definition of serverless computing
How can serverless computing be defined and how can it be distinguished from a kubernetes based PaaS offering?

## Task 2 - Install knative on your kubernetes cluster
Recommendation: Use minikube or kind to be able to use kn's quickstart plugin

a) Install knative on your kubernetes cluster.

b) Create a knative service with a webapp of your choice and request it by using an http-client

## Task 3 - Cold-start-delay
a) Describe the cases in which a cold-start-delay can occur.

b) Create a service in knative that uses the standard knative pod autoscaler and for which the occurrence of cold-start-delays is impossible.

c) Wait one minute to simulate that the service didn't receive any traffic for one minute. Verify that no cold start occurs when sending a request to the service.

## Task 4 - Revisions and Traffic Splitting
a) Create a knative service responding with http status code 200 to all http GET requests.

b) Create a new revision of this knative service, which responds with http status code 201 to all http GET requests.

c) Let knative split the incoming traffic 40% to the first revision and 60% to the second revision.

d) Test the traffic splitting functionality by using a load generator. (For example, use the tool "hey").

## Task 5 - Autoscaling
a) Create a knative service with a target concurrency of 10 concurrent requests per replica. Use the default Knative Pod Autoscaler.

b) Let the new service receive 50 concurrent http-requests from a load generator (for example use the tool "hey").

c) You will see that the autoscaling algorithm does not scale the number of replicas to exactly 5 as expected. How many replicas do you see?

d) Describe the reason for the observed behavior and change the configuration so that you see exactly 5 replicas for 50 concurrent requests.

## Task 6 - Domains
a) Configure that requesting the domain stable.example.de via knative's ingress forwards traffic to the service from task 4 (no https needed)

b) Verify the correct functionality of the domain mapping with an http-client.

## Task 7 - Knative Eventing
a) Create a new knative eventing broker, which has the type "Multi-tenant channel-based broker".

b) Create a trigger listening for events on the new broker, which forwards CloudEvents to one of your already created knative services.

* The trigger shall only react to CloudEvents having a type with a value of your choice.

d) Verify the correct functionality of the trigger by sending a CloudEvent to your broker.
* (You need to send the event from a pod inside the kubernetes cluster to reach the broker url, or you will get http status code 404 from the ingress controller. I recommend using curl to send a simple event from a pod within the cluster)