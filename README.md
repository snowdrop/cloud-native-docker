# Instructions to create app, build it

- Git clone project
  
  ```bash
  git clone https://github.com/snowdrop/ocp-docker-build-install && cd ocp-docker-build-install
  ```

- Log on to OpenShift and create project

  ```bash
  oc login $(minishift ip):8443 -u admin -p admin
  oc new-project ocp-docker-build-install
  ```

- Build locally

  ```bash
  mvn clean package
  rm -rf target/*-1.0.jar
  ```
  
- A buildConfig resource is created to process the local Dockerfile and build the docker image of the app
  ```bash
  oc new-build --binary --strategy=docker --name=docker-build -l app=ocp-docker-build
  oc start-build docker-build --from-dir=. --follow
  ```

- The application is created on the cloud platform
  ```bash
  oc new-app --name=docker-build -i docker-build -l app=ocp-docker-build  
  ```

- Clean up the resources

  ```bash
  oc delete dc,bc,is,svc docker-build
  or
  oc delete all -l app=ocp-docker-build 
  ```
  
# Using generated/static files

  ```bash
  mvn clean package
  rm -rf target/*-SNAPSHOT.jar
  oc create -f generated/app.yaml
  oc start-build docker-build --from-dir=. --follow
  ```  