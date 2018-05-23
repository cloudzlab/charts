# ZCP Registry

## Helm package

```
git clone https://github.com/cnpst/zcp-registry.git
helm dependency build zcp-registry
helm package zcp-registry

git clone https://cnpst.github.io/charts
mv zcp-registry-x.x.x.tgz charts/docs
helm repo index charts/docs --url https://cnpst.github.io/charts
```

## Add Helm repository

```
# helm repo add zcp https://cnpst.github.io/charts

# helm repo list
NAME     	URL
...
zcp      	https://cnpst.github.io/charts

# helm search zcp-registry
NAME        	    VERSION	 DESCRIPTION
zcp/zcp-registry  	x.x.x  	 xxx
```

## Deploy ZCP Registry Helm Chart

1. Edit values.yaml

    ```
    # The FQDN for ZCP Registry service.
    externalDomain: registry.example.com

    # ZCP Registry FQDN as insecure-registries for your docker client.
    insecureRegistry: false

    # The TLS certificate for ZCP Registry. The common name of tlsCrt must match the externalDomain above.
    tlsCrt: |
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----

    tlsKey: |
      -----BEGIN RSA PRIVATE KEY-----
      ...
      -----END RSA PRIVATE KEY-----

    adminserver:
      adminPassword: Registry12345
    ```

2. Deploy

    ```
    helm install --namespace=zcp-registry --name zcp-registry -f values.yaml zcp/zcp-registry
    ```

3. Optional

    3.1. To use Persistent Volume

    ```
    ## This will be applied to global except for postgresql
    persistence:
      enabled: true

    adminserver:
      ...
      volumes:
        config:
          storageClass: "STORAGE_CLASS_NAME"
          accessMode: ReadWriteOnce
          size: 20Gi

    mysql:
      ...
      volumes:
        data:
          storageClass: "STORAGE_CLASS_NAME"
          accessMode: ReadWriteOnce
          size: 20Gi

    registry:
      ...
      volumes:
        data:
          storageClass: "STORAGE_CLASS_NAME"
          accessMode: ReadWriteOnce
          size: 20Gi

    postgresql:
      ...
      persistence:
        enabled: true
        storageClass: "STORAGE_CLASS_NAME"
        accessMode: ReadWriteOnce
        size: 20Gi

    ```

    3.2. To user more client-max-body-size

    ```
    ingress:
      annotations:
        ...
        nginx.org/client-max-body-size: "900m"
    ```

    3.3. To change POD's resource

    ````
    adminserver:
      ...
      resources:
        requests:
          memory: 256Mi
          cpu: 100m

    ui:
      ...
      resources:
        requests:
          memory: 256Mi
          cpu: 100m

    mysql:
      ...
      resources:
        requests:
          memory: 256Mi
          cpu: 100m

    registry:
      ...
      resources:
        requests:
          memory: 256Mi
          cpu: 100m

    clair:
      ...
      resources:
        requests:
          memory: 256Mi
          cpu: 100m

    ```
