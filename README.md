# Instructions to create app, build it

```bash
mvn clean package
rm -rf target/*-SNAPSHOT.jar

oc new-build --binary --strategy=docker --name=docker-build -l app=cloud-docker-build
oc start-build docker-build --from-dir=. --follow
oc new-app --name=docker-build -i docker-build -l app=cloud-docker-build
```

# Using generated/static files

```bash
mvn clean package
rm -rf target/*-SNAPSHOT.jar
oc create -f generated/app.yaml
oc start-build docker-build --from-dir=. --follow
```

## Clean up

```bash
oc delete dc,bc,is,svc docker-build
or
oc delete all -l app=cloud-docker-build 
```