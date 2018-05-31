# Instructions to create app, build it

```bash
mvn clean package
rm -rf target/*-SNAPSHOT.jar

oc new-build --binary --strategy=docker --name=docker-build -l app=docker-build
oc start-build docker-build --from-dir=. --follow
oc new-app --name=docker-build -i docker-build
```

## Clean up

```bash
oc delete dc,bc,is,svc docker-build
```