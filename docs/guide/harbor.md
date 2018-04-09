# Harbor

## Helm package

```
git clone https://github.com/cloudzlab/charts.git
cd charts
helm dependency build harbor
helm package harbor
mv harbor-x.x.x.tgz docs
helm repo index docs --url https://cloudzlab.github.io/charts
```

## Add Helm repository

```
# helm repo add zcp https://cloudzlab.github.io/charts

# helm repo list
NAME     	URL
...
zcp      	https://cloudzlab.github.io/charts

# helm search harbor
NAME        	VERSION	DESCRIPTION
zcp/harbor  	0.1.1  	An Enterprise-class Docker Registry by VMware
```

## Deploy Harbor Helm Chart

1. Edit values.yaml

    ```
    # The FQDN for Harbor service.
    externalDomain: harbor.com
    ```

2. Deploy

    ```
    helm install --namespace=harbor --name harbor -f values.yaml zcp/harbor
    ```
