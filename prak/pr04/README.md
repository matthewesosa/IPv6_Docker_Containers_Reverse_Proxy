# Task 1 - remote deployment of miniwhoami

## (1b) Create miniwhoami a tagged Docker registry for your web application Dockerimage . Which docker command can you use for this?
* docker login docker.fslab.de
* docker tag migbin2s/miniwhoami:latest docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami:latest

## (1c) Use the app miniwhoami to push your image up to the registry at https://git.fslab.de .
* docker push docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami:latest

## (1d) Deploy docker run a container of miniwhoami_20411 your web application miniwhoami to your lab server serv-ws22. Use the port for this 20411. Which command do you use for this?
* docker pull docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami
*  docker run -p 20411:5000 --name miniwhoami_20411 --restart=always -d docker.fslab.de/migbin2s/servmgmt-ws22/miniwhoami