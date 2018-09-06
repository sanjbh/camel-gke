FROM openjdk:8u102-jre
ARG finalName
ADD $finalName /my-app.jar
ENTRYPOINT ["java","-jar","/my-app.jar"]
