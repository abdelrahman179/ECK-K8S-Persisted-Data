Elastic Cloud with Kibana deployment on Kubernetes cluster considering data persistence using Local Path Provisioner


# Please note that elasticsearch along with are deployed in "elasticsearch" namespace
# Elastic-search data are persisted in "/mnt/elastic-search" dir in all nodes > default


# ---- Request Elasticsearch access
# get the ClusterIP service which is automatically created for your cluster:
kubectl get service elasticsearch-es-http


# get the credentials
# A default user named elastic is automatically created with the password stored in a Kubernetes secret:
PASSWORD=$(kubectl get secret elasticsearch-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')


# From your local workstation, use the following command in a separate terminal:
kubectl port-forward service/elasticsearch-es-http 9200


curl -u "elastic:$PASSWORD" -k "https://localhost:9200"


# ---- Access Kibana
# A ClusterIP Service is automatically created for Kibana:
kubectl get service elasticsearch-kb-http

# get the elastic password
kubectl get secret elasticsearch-es-elastic-user -n elasticsearch -o=jsonpath='{.data.elastic}' | base64 --decode; echo

kubectl port-forward service/kibana-kb-http -n elasticsearch 5601

# open a new window in browser and type https://localhost:5601
# Login as the elastic user. The password can be obtained with the following command:

User: elastic
Password: ni37qp2A68A3Qjc9PC5Hyy43
