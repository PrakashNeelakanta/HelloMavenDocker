# HelloMavenDocker
Docker web project


Docker web project which contains oracle-java 7, maven, tomcat web server.

How to run

$ docker build -t simplewebapp .

$ docker run --name webapp -p 8080:8080 simplewebapp

now after this if you open http://localhost:8080/webAppTest 
result 

"Hello World"
