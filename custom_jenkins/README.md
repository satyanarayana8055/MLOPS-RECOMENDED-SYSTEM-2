docker build -t jenkins-dind . 

docker images

it will containerize the image
docker run -d \
  --name jenkins-dind \
  --privileged \
  -p 8080:8080 \
  -p 50000:50000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v jenkins_home:/var/jenkins_home \
  jenkins-bind

docker logs jenkins-bind
it gives inital pass to setup the jenkins

docker exec -u root -it jenkins-dind bash
