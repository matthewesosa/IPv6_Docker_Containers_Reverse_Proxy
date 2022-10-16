#initialize a base image
FROM python:3.8.10-alpine
# define the present working directory
WORKDIR /miniwhoami
# copy the contents into the working dir
ADD . /miniwhoami
# run pip to install the dependencies of the flask app
RUN apk add build-base linux-headers
RUN python -m pip install psutil
RUN pip install -r requirements.txt
# define the command to start the container
CMD ["python","miniwhoami.py"]
