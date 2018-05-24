# ZCP Catalog

## Helm package

```
git clone https://github.com/cnpst/zcp-catalog.git
helm dependency build zcp-catalog
helm package zcp-catalog

git clone https://cnpst.github.io/charts
mv zcp-catalog-x.x.x.tgz charts/docs
helm repo index charts/docs --url https://cnpst.github.io/charts
```

## Add Helm repository

```
# helm repo add zcp https://cnpst.github.io/charts

# helm repo list
NAME     	URL
...
zcp      	https://cnpst.github.io/charts

# helm search zcp-catalog
NAME        	    VERSION	 DESCRIPTION
zcp/zcp-catalog  	x.x.x  	 xxx
```

## Deploy ZCP Catalog Helm Chart

1. Edit values.yaml

    ```
    api:
      replicaCount: 1

    ui:
      appName: ZCP Catalog

    ingress:
      enabled: true
      hosts:
        - catalog.example.com
      annotations:
        ingress.kubernetes.io/rewrite-target: /  
      tls:
        secretName: catalog-tls

    mongodb:
      persistence:
        enabled: false
    ```

2. Deploy

    ```
    helm install --namespace=zcp-catalog --name zcp-catalog -f values.yaml zcp/zcp-catalog
    ```

3. Optional

    - To use Persistent Volume

    ```
    mongodb:
      persistence:
        enabled: true
        storageClass: "STORAGE_CLASS_NAME"
        accessMode: ReadWriteOnce
        size: 20Gi
    ```

    - To change POD's resource

    ````
    api:
      resources:
        limits:
          cpu: 100m
          memory: 128Mi
        requests:
          cpu: 100m
          memory: 128Mi

    ui:
      resources:
        limits:
          cpu: 100m
          memory: 128Mi
        requests:
          cpu: 100m
          memory: 128Mi

    prerender:
      resources:
        requests:
          cpu: 100m
          memory: 128Mi
    ```
