# kubernetisCronJob
This example is only for linux user.
For running this on your local please install minikube - https://minikube.sigs.k8s.io/docs/start/
https://www.javatpoint.com/docker-java-example
https://www.twilio.com/blog/automate-scripts-golang-minikube-cronjobs

A complete example on how to run Kubernetis Cron Job 
*** download the file in a folder or create yourself. naviate to the folder on terminal/console

1. create a helloWorld.java program

	class HelloWorld{  
		public static void main(String[] args){  
			System.out.println("This is java app \n by using Docker");  
		}  
	}
2. Create a file with name "Dockerfile" and save

	FROM java:8  
	COPY . /var/www/java  
	WORKDIR /var/www/java  
	RUN javac HelloWorld.java  
	CMD ["java", "HelloWorld"] 
3. start minikube
	minikube start
4. set minikube environment variable
	eval $(minikube docker-env)
5. navigate to the folder where "Dockerfile" is saved
6. build docker image( Run the below command from the folder where Dockerfile is saved to build docker image)
	docker build -t hello . -f Dockerfile
	*** hello us name of docker image build
7. execute the below command to run/start the docker image. 
	sudo docker run java-app
	The "java-app" is docker image name. It will start the docker and run the program within it.
	
8. remove docker image
	sudo docker rmi image imageid
		
---------------------------------------- How to run cronjob in minicube ----------------------
9. Create a cronjob yml file

	----------------------------------------------------------------------
	apiVersion: batch/v1beta1
	kind: CronJob
	metadata:
	  creationTimestamp: null
	  name: simplejob
	spec:
	  jobTemplate:
	    metadata:
	      creationTimestamp: null
	      name: simplejob
	    spec:
	      template:
		metadata:
		  creationTimestamp: null
		spec:
		  containers:
		  - image: hello:latest
		    imagePullPolicy: IfNotPresent
		    name: simplejob
		    resources: {}
		  restartPolicy: OnFailure
	  schedule: '*/1 * * * *'
	status: {}
	----------------------------------------------------------------------

**** image is the name of docker image complied and creaed previousely
**** schedule is the time when it will keep on auto running
**** nake sure the eval command eval "$(minikube docker-env)" is run before building the docker image

10 kubectl apply -f job.yaml
11. On success it will show below message
	CronJob.batch/simplejob created
12. kubectl get pod --watch
	It will list all pods running
13. to see the detail log of POD run the below command

	kubectl logs simplejob-1597939920-dchqd
	****	simplejob-1597939920-dchqd is name of running pod
14. delete the cronjob
	kubectl delete CronJob simplejob

	**** simplejob is name of cronjob
