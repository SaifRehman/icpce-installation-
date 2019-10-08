## Docker Push transformation-advisor-db:1.9.9
```
wget https://ibm.box.com/shared/static/i4bu7wtlcpkdt3ewm255z8dhecfmtgi2.gz
docker load < transformation-advisor-db.gz
docker tag ibmcom/transformation-advisor-db:1.9.9 mycluster.icp:8500/transformation-advisor-db:1.9.9
docker push mycluster.icp:8500/transformation-advisor-db:1.9.9
```

## ibmcom/transformation-advisor-server
```
wget https://ibm.box.com/shared/static/gicep7odeyd3gwjy3s54nsorj3dc1b8z.gz
docker load < transformation-advisor-server.gz
docker tag ibmcom/transformation-advisor-server:1.9.9 mycluster.icp:8500/transformation-advisor-server:1.9.9
docker push mycluster.icp:8500/transformation-advisor-server:1.9.9
```

## ibmcom/transformation-ui
```
wget https://ibm.box.com/shared/static/39aydw1qphes4v291ciu0r7yfxz6t3gt.gz
docker load < transformation-advisor-ui.tar.gz
docker tag ibmcom/transformation-advisor-ui:1.9.9 mycluster.icp:8500/transformation-advisor-ui:1.9.9
docker push mycluster.icp:8500/transformation-advisor-ui:1.9.9
```
## Create secret
```
kubectl -n default create secret generic transformation-advisor-secret --from-literal=db_username='YWRtaW4=' --from-literal=secret='YWRtaW4='
```

## install chart 

```
unzip the file and run
helm install --name my-release --set authentication.icp.edgeIp=<edgeIP> --set authentication.icp.secretName=<my-secret> ibm-transadv-dev/ --tls
```
