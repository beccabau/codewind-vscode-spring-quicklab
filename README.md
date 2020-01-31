# Codewind Quicklab

Moving workloads to the cloud introduces a steep learning curve for developers accustomed to traditional web application developmenr. Cloud-native applications are usually containerized (either Docker or OCI containers) and are run on orchestrated platforms like Kubernetes. These are complex technologies that bring big changes to the development workflow. Can this be simplified? Can developers create cloud-native applicatioons and deploy them to Kubernetes without climbing the mountainous learning curve? 

In this quicklab we will take a look at an usecase involving Eclipse Codewind, a technology that simplify development of containerized cloud-native applications. Codewind is an IDE plugin that allows you to work with containerized applications in a familiar way. With Codewind, developers can start building cloud-native applications like experts without having to be an expert in all the cloud-native technologies. 

## Prerequistes

If you're running this lab on your own laptop, make sure you have a few things already installed.

<details>
  <summary>Click to expand</summary>
  
### Configure Local System

This quicklab requires the following tools: 

1. Docker
2. VS Code
3. VS Code Codewind [extension](https://www.eclipse.org/codewind/mdt-vsc-getting-started.html)

We recommend working with the latest available version of each.

</details>

## Improving Developer Productivity with Eclipse Codewind

[Eclipse Codewind](https://www.eclipse.org/codewind/) is a plugin for IDEs, currently available in VS Code, Eclipse, and Eclipse Che, that helps improve developer productivity when developing containerized applications. Let's explore how Codewind can help you be a more productive developer.

1. Open VS Code (press **command** + **space bar** and type "VS Code" into the dialog box)
2. In the explorer window under **CODEWIND** click on the "**+**" to create a new project
![](images/codewind-explorer.png)

3. In the dialog pop-up search for "Spring Boot" and select the "Appsody Spring Boot default template" option
![](images/Create-cw-spring-boot.png)

4. Enter **cloud-native-spring** as the project name and hit enter
![](images/codewind-explorer-new.png)

### Automated Code Reload

A key to increasing developer productivity is shortening and reducing the friction in the feedback loop. Codewind improves your inner loop experience, enabling you to create a microservice quickly, rapidly iterate on changes and make improvements to performance. As you develop, Codewind *automatically* pushes your code changes to your container as efficiently as possible. 

Let's look at this feature in action.

4. 	In the project under **src/main/java/application** create a new file **Hello.java**
![](images/create-hello-dot-java.gif)
5. Edit **Hello.java** to look like below:
	
	```java
	package application;
	
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RestController;
	import org.springframework.web.bind.annotation.RequestParam;
	import org.springframework.http.HttpStatus;
	import org.springframework.http.ResponseEntity;
	import org.springframework.web.bind.annotation.ResponseBody;
	import java.util.ArrayList;
	import java.util.List;
	
	@RestController
	public class Hello {
	
	    @RequestMapping("v1/hello")
	    public @ResponseBody ResponseEntity<String> example(@RequestParam("name") String name) {
	        List<String> list = new ArrayList<>();
	        //return a simple list of strings
	        String msg = "Hello " + name;
	        list.add(msg);
	        System.out.println("New message: " + msg);
	        return new ResponseEntity<String>(list.toString(), HttpStatus.OK);
	    }
	
	}
	```
6. You can view the status of the re-build and re-deploy by looking at the status indicator next to the project under the Codewind context. Once status returns to [Running][Build Suceeded] you can refresh your browser window to view the change we made. Please be aware that it can take a few seconds until something happens. 
	![](images/app-status.JPG)	
1. In VS Code click the "Open Application" icon	
2. Append `/v1/hello?name=Cloud%20Native%20Spring` to the end of the url
![](images/append-url.gif)

### Viewing Application Logs

Viewing the logs of your application running in a docker container is easy from the IDE with Codewind. 

To view the logs for an application, right click on it and select "Show All Logs" 

![](images/show-logs-new.png)

The logs for the running application will be shown in the IDE console log window on the bottom right of the page. 

### Deploying an Application to Kubernetes with Appsody 

The cloud native world demands developer learn a lot of new skills that traditionally they didn't need understand before. Appsody helps to reduce this learning curve by helping with tasks like deploying to a kubernetes cluster. Let's deploy the application we have been building in this quicklab to a local kubernetes cluster!

1. Make sure that you are in the correct folder before starting your minikube cluster: 

	```
	cd “~/codewind-workspace/Cloud Native Spring”
	```

1. Now we will need to start our minikube cluster:

	```
	minikube start
	```

1. Next we will need to setup docker registry for the local minikube cluster, so that minikube can pull the image we will be sending it in the next step. To do that run the following command:

	```
	eval $(minikube docker-env)
	```

1. To deploy the application we just created to a kubernetes cluster, in this case a locally running instance of [minikube](https://github.com/kubernetes/minikube), run the following command:

	```
	appsody deploy
	``` 

1. Once the deploy has completed, we will need to tell minikube to expose the service, to do this run the following command:

	```
	minikube service cloud-native-spring
	```

Minikube will expose the service and open a browser window allowing you to view the application we just deployed.

Appsody and Codewind help Java developers create, build, and deploy cloud-native applications without having to be experts in cloud native development!

