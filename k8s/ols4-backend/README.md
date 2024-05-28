# Install ols4-semlookp version

### Example:
```
helm install ols4 \
--set-json='ingress.dns="ols4.qa.km.k8s.zbmed.de"'  \
--set-json='backend.backendImage="ghcr.io/ebispot/ols4-backend:stable"'  \
--set-json='backend.solrImage="ghcr.io/ebispot/ols4-solr:9.0.0"'  \
--set-json='backend.neo4jImage="ghcr.io/ebispot/ols4-neo4j:4.4.9-community"'  \
--set-json='backend.context="/ols4"'  \
--set-json='backend.neo4jTarballUrl="http://ols4-dataserver/neo4j.tgz"'  \
--set-json='backend.solrTarballUrl="http://ols4-dataserver/solr.tgz"'  \
ols4/k8chart/ols4/
```