


### GITHUB
In the github I used the repo https://github.com/M4gOo/hello-world-MAVEN-Project1
I had to create a token, then jenkins can have access (usually when it is private repo). In my case, I did just to practice.



### VMs
I installed java, maven and git packages controller and slave(webserver)

Create user jenkins on slave and add the public_jenkins_controller ssh

![create a user jenkins - the controller can access jenkins slave using ssh](https://user-images.githubusercontent.com/57456345/215891129-f4f1ed69-830d-4b92-aa58-de40ffa38a1b.JPG)

in the slave install and configure tomcat

to install follow the link below

https://github.com/M4gOo/Hands-on/blob/developer-roadmap/Playbook%20-%20Install%20tomcat


nano /usr/local/tomcat/apache-tomcat-8.5.85/webapps/host-manager/META-INF/context.xml

comment this part using  !-- at the begin and -- at the end. 

  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
         
![comment](https://user-images.githubusercontent.com/57456345/215912147-14a943e6-75be-48fb-8bda-8633ef8a5223.JPG)

also configure user to access tomcat webpage. That credential it will be in the post build action inside the container under credentials
the user must have the role=manager-script

nano /usr/local/tomcat/apache-tomcat-8.5.85/conf/tomcat-users.xml

<role rolename="tomcat"/>
<user username="tomcat" password="admin" roles="tomcat,manager-gui,admin-gui,manager-script"/>


change the port because jenkins is open on 8080 and tomcat as well. To change tomcat server port

nano /usr/local/tomcat/apache-tomcat-8.5.85/conf/server.xml


   <!-- Define an AJP 1.3 Connector on port 8009 -->
    <!--
    <Connector protocol="AJP/1.3"
               address="::1"
               port="8009"
               redirectPort="8443" 


### JENKINS

Configure global settings, select the same version installed in the slave and controller

![checkmavenservers](https://user-images.githubusercontent.com/57456345/215903604-5737d6fe-16f8-4ddc-960f-453759443b00.JPG)

![globalconfmaven](https://user-images.githubusercontent.com/57456345/215903509-c725d17f-5d1b-4ae2-bb37-303a55a3cc97.JPG)


Cofigure the slave, where the application it will be running. 

![slave configuration](https://user-images.githubusercontent.com/57456345/215903904-572989cb-7709-4354-a509-258e54d19894.JPG)

Download the plugin maven

![maven plugin](https://user-images.githubusercontent.com/57456345/215889289-c2eb5d2d-7cf4-46d4-83f5-d1c6662f7d86.JPG)

Download the deploy to a container

![container deploy](https://user-images.githubusercontent.com/57456345/215902825-0c30629a-4029-4e55-9d88-c092b964610e.JPG)


Then create new item using the plugin maven

![newitemmaven](https://user-images.githubusercontent.com/57456345/215889748-dedc0598-b1bb-4668-a1ca-cfdcd996237a.JPG)


The configuration for the projecct it will be

![configuretorunintheslave](https://user-images.githubusercontent.com/57456345/215890392-6a92c16e-bf6e-47be-8ea2-24be862264ae.JPG)

![githubaccess](https://user-images.githubusercontent.com/57456345/215890475-804fc2f1-e91d-43f4-a8bd-a78ead3f8319.JPG)


pom.xml comes with the code

![pox install maven](https://user-images.githubusercontent.com/57456345/215890765-f6dcc4be-d35e-43b1-87dd-034a8681ce2d.JPG)

Select the plugin "deploy to a container" to deploy in the slave server. 

![selectposbuildtodeploy](https://user-images.githubusercontent.com/57456345/215890698-6694f5eb-84e6-4561-93d5-f1c3269b6319.JPG)

![posbuild](https://user-images.githubusercontent.com/57456345/215890569-e5f99243-ef8e-4df2-87cb-ae5a079de663.JPG)


#build now. After some fails I manae to deploy in the slave

![nice](https://user-images.githubusercontent.com/57456345/215910118-bd8b066d-36d9-4a57-a62c-1cfb253ea375.JPG)

![nice2](https://user-images.githubusercontent.com/57456345/215910169-69cbec70-33d1-4be7-b91a-607ce5cdce33.JPG)

In case you want to change the text that it will shown in tomcat
https://github.com/M4gOo/hello-world-MAVEN-Project1/blob/master/webapp/src/main/webapp/index.jsp 

![nice3](https://user-images.githubusercontent.com/57456345/215910235-29628c08-6148-4833-bffc-4597cb11032b.JPG)









