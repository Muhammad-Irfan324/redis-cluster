# USAGE

- Building docker image with tag

docker build -t jenkins-docker .

- Running jenkins-docker container with docker sock and .kube directory mount and port mapping
  
docker run -d --name jenkins -restart=unless-stopped -p 8080:8080  -v ~/.kube:/var/jenkins_home/.kube -v jenkins_data:/var/jenkins_home  -v /var/run/docker.sock:/var/run/docker.sock jenkins-docker:latest