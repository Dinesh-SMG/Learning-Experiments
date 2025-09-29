pipeline {
    agent { label 'Maven' } // Runs on the node/agent labeled 'Maven'

    tools {
        maven 'Maven3' // Replace with your exact Maven installation name in Jenkins
    }

    environment {
        TOMCAT_URL = 'http://54.89.151.231:8080'
    }

    stages {

        stage('Prepare Project') {
            steps {
                // Create project structure
                sh '''
                mkdir -p CurrentTimeApp/src/main/java/com/example
                mkdir -p CurrentTimeApp/src/main/webapp/WEB-INF
                mkdir -p CurrentTimeApp/src/main/webapp
                '''

                // Create pom.xml
                writeFile file: 'CurrentTimeApp/pom.xml', text: '''<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>CurrentTimeApp</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    <build>
        <finalName>CurrentTimeApp</finalName>
    </build>
</project>'''

                // Create Java Servlet
                writeFile file: 'CurrentTimeApp/src/main/java/com/example/CurrentTimeServlet.java', text: '''package com.example;

import java.io.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import javax.servlet.*;
import javax.servlet.http.*;

public class CurrentTimeServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {

        response.setContentType("text/html");
        LocalDateTime now = LocalDateTime.now();
        String formattedTime = now.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));

        PrintWriter out = response.getWriter();
        out.println("<html><body>");
        out.println("<h1>Current Server Time:</h1>");
        out.println("<p>" + formattedTime + "</p>");
        out.println("</body></html>");
    }
}'''

                // Create web.xml
                writeFile file: 'CurrentTimeApp/src/main/webapp/WEB-INF/web.xml', text: '''<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>
        <servlet-name>CurrentTimeServlet</servlet-name>
        <servlet-class>com.example.CurrentTimeServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>CurrentTimeServlet</servlet-name>
        <url-pattern>/time</url-pattern>
    </servlet-mapping>

</web-app>'''

                // Optional: index.jsp
                writeFile file: 'CurrentTimeApp/src/main/webapp/index.jsp', text: '''<html>
<body>
<h2>Welcome to Current Time App</h2>
<p>Click <a href="time">here</a> to see the current server time.</p>
</body>
</html>'''
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'cd CurrentTimeApp && mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat8(
                    credentialsId: 'tomcat-user',
                    path: '',
                    url: "${env.TOMCAT_URL}"
                )], contextPath: 'CurrentTimeApp', war: 'CurrentTimeApp/target/*.war'
            }
        }
    }

    post {
        success {
            echo "✅ Build & Deployment Successful! Visit ${env.TOMCAT_URL}/CurrentTimeApp/time"
        }
        failure {
            echo "❌ Build or Deployment Failed!"
        }
    }
}
