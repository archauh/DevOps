🚀 Deploy a Java Maven WebApp to Apache Tomcat and Push to GitHub
🧰 Prerequisites
Install the required tools:
# Update packages
sudo apt update

# Install Java
sudo apt install default-jdk -y

# Install Maven
sudo apt install maven -y


📦 Install and Configure Apache Tomcat

cd /opt
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.42/bin/apache-tomcat-10.1.42.tar.gz

Extract the tar file


tar xzf apache-tomcat-10.1.42.tar.gz
mv apache-tomcat-10.1.42 tomcat
chmod +x /opt/tomcat/bin/*.sh
/opt/tomcat/bin/startup.sh


Access Tomcat at:
👉 http://server-ip:8080

📁 Create Sample Java WebApp with Maven


mvn archetype:generate -DgroupId=com.example -DartifactId=sample-webapp \
-DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false

cd sample-webapp


🛠 Update pom.xml

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>sample-webapp</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <name>Sample WebApp</name>
    <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    <build>
        <finalName>sample-webapp</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.4.0</version>
            </plugin>
        </plugins>
    </build>
</project>



🧾 Create a Simple Servlet

mkdir -p src/main/java/com/example
nano src/main/java/com/example/HelloServlet.java


package com.example;
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class HelloServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        resp.getWriter().write("Hello from Maven WebApp deployed on Tomcat!");
    }
}


⚙️ Configure web.xml


mkdir -p src/main/webapp/WEB-INF
nano src/main/webapp/WEB-INF/web.xml

<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         version="3.0">
    <servlet>
        <servlet-name>hello</servlet-name>
        <servlet-class>com.example.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>hello</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>


🏗️ Build the WAR File
mvn clean package


Output:
target/sample-webapp.war


🚀 Deploy WAR to Tomcat
sudo cp target/sample-webapp.war /opt/tomcat/webapps/

/opt/tomcat/bin/shutdown.sh
/opt/tomcat/bin/startup.sh

Access the application:
http://<your-server-ip>:8080/sample-webapp/



