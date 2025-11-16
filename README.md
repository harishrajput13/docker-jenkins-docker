# Docker-Jenkins-Docker
Project Discription:-
Install Docker, run Jenkins inside a Docker container, and then use that Jenkins container to create a pipeline that pulls an image from Docker Hub and launches a new container from that image.

Setup Docker and Jenkins (with master & slave env)
Step1: create an EC2 instance named as "docker-jenkins", login into the "docker-jenkins" via SSH
Step2: install Docker engine and pull image of jenkins container and run a container of jenkins with that image. 
Step3: allow SSH and 8080 port in the security group of EC2 istance "docker-jenkins"
Step4: copy the public IP of the instance run on ay browser like "http://<public_ip>:8080" and hit the jenkins login page and login into jenkins
Step5: now create a 2nd EC2 instance names ad "jenkins-slave1" and install docker and java inside the instance 
Step6: now configure the slave (worker node), go to "manage jenkins" --> go to "nodes" --> give the root dir "/home/ubuntu", give label to your node "slave1", uage  
choose "only build jobs with labels" option, launch method choose "launch via SSH", in host add the "private ip on instance "jenkins-slave1"", credentials "add" -->  
"jenkins" -->  kind choose "SSH with private key", give ID name, add the instance username "ubuntu", and add the private key data , ADD --> after adding the  
credential select from drop down "ID name", host key strategy choose "Non veryfing strategy" & Save.

[Note: make sure that the java version of your master and slave should be same version ]

Step7: Now go to jnekins home page, create a job --> name the job "pipeline-pull-image", select the type "Pipeline", go to pipeline section add the groovy syntax  
from the (Jenkinsfile) that I have given in jenkinsfile/Jenkinsfile copy the code and paste the code in the pipeline section, save the changes 
Step8: Build the pipeline
Step9: make sure that in the jenkinsfile groovy syntax the agent label must match with slave label.
