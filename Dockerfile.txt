FROM maven:3.9.11-eclipse-temurin-11-alpine AS build
MAINTAINER kvsatishkumar7
LABEL org.opencontainers.image.authors="kvsatishkumar7"
WORKDIR /opt/demo
COPY . /opt/demo
RUN mvn package -DskipTests

FROM tomcat:jre8-openjdk-slim-buster AS run
WORKDIR /usr/local/tomcat
COPY --from=build /opt/demo/target/sysfoo.war webapps/ROOT.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
