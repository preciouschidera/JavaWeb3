# Use an official Java runtime as a parent image
FROM tomcat:9-jdk11-openjdk-slim

# Set the working directory
WORKDIR /usr/local/tomcat/webapps/

# Copy the WAR file into the container
COPY target/WebAppCal-0.0.6.war /usr/local/tomcat/webapps/ROOT.war

# Expose the port
EXPOSE 8080

# Start Tomcat
CMD ["catalina.sh", "run"]


