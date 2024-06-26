# Install ols4-semlookp version

### Example:
```
helm install ols4 \
--set-json='ingress.dns="ols4.qa.km.k8s.zbmed.de"'  \
--set-json='backend.backendImage="ghcr.io/ebispot/ols4-backend:stable"'  \
--set-json='solrImage="ghcr.io/ebispot/ols4-solr:9.0.0"'  \
--set-json='neo4jImage="ghcr.io/ebispot/ols4-neo4j:4.4.9-community"'  \
--set-json='backend.context="/ols4"'  \
--set-json='neo4jTarballUrl="http://ols4-dataserver/neo4j.tgz"'  \
--set-json='solrTarballUrl="http://ols4-dataserver/solr.tgz"'  \
ols4-backend-deployment/ols4-backend
```

Add this for enabling certification (certIssuer must be installed): 
```bash
--set-json='ingress.enableSSL="true"'  \
--set-json='ingress.certIssuer="letsencrypt-prod"'  \
```
