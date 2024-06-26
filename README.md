# OLS4 backend deployment
OLS4 backend - Deployment Configuration


## Deploy OLS4 backend

### Generate data

Enter the OLS4 project. Do as described [here](https://github.com/EBISPOT/ols4?tab=readme-ov-file#deploying-ols4).

In short:
- copy the OWL or RDFS ontology file to the testcases folder
- Then make a new config file for your ontology in dataload/configs (you can use efo.json as a template)
- For the ontology_purl property in the config, use e.g. file:///opt/dataload/testcases/myontology.owl if your ontology is in testcases/myontology.owl

```bash
export OLS4_CONFIG=./dataload/configs/meshd-v1.json
docker compose up
```

The data is generated and saved locally in /var/lib/docker/volumes/<your_project_name>/_data

### Create data archives for Solr and Neo4j
```bash
sudo tar --use-compress-program="pigz --fast --recursive" -cf /neo4j.tgz -C /var/lib/docker/volumes/<your project name>/_data .
sudo tar --use-compress-program="pigz --fast --recursive" -cf /solr.tgz -C /var/lib/docker/volumes/<your project name>/_data .
```

Eventually, the permissions need to be changed:
```bash
sudo chown -R username:username /neo4j.tgz
sudo chown -R username:username /solr.tgz
```

### Deploy ols4-dataserver
```bash
helm repo add ols4-backend-deployment https://ts4nfdi.github.io/ols4-backend-deployment/
helm install ols4-dataserver ols4-backend-deployment/ols4-dataserver
```

### Upload data to ols4-dataserver
```bash
kubectl cp /neo4j-meshd-v1.tgz ols4-dataserver-7d9d88cd86-n98fb:/usr/share/nginx/html/neo4j.tgz
kubectl cp /solr-meshd-v1.tgz ols4-dataserver-7d9d88cd86-n98fb:/usr/share/nginx/html/solr.tgz
```

### Deploy ols4-backend
```bash
helm install <your release name> \
--set-json='ingress.dns="<your domain>"'  \
--set-json='backend.backendImage="ghcr.io/ebispot/ols4-backend:stable"'  \
--set-json='solrImage="ghcr.io/ebispot/ols4-solr:9.0.0"'  \
--set-json='neo4jImage="ghcr.io/ebispot/ols4-neo4j:4.4.9-community"'  \
--set-json='backend.context="/ols4"'  \
--set-json='neo4jTarballUrl="http://ols4-dataserver/neo4j-nfdi4health.tgz"'  \
--set-json='solrTarballUrl="http://ols4-dataserver/solr-nfdi4health.tgz"'  \
ols4-backend-deployment/ols4-backend
```