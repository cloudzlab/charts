# ZCP Elasticsearch exporter

## Helm package

```
git clone https://github.com/cnpst/elasticsearch-exporter.git
helm package elasticsearch-exporter

git clone https://cnpst.github.io/charts
mv elasticsearch-exporter-x.x.x.tgz charts/docs
helm repo index charts/docs --url https://cnpst.github.io/charts
```

## Add Helm repository

```
# helm repo add zcp https://cnpst.github.io/charts

# helm repo list
NAME     	URL
...
zcp      	https://cnpst.github.io/charts

# helm search elasticsearch-exporter
NAME        	    		VERSION	 DESCRIPTION
zcp/elasticsearch-exporter  	x.x.x  	 xxx
```
