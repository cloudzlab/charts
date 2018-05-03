# Harbor

## Helm package

```
git clone https://github.com/cnpst/charts.git
cd charts
helm dependency build harbor
helm package harbor
mv harbor-x.x.x.tgz docs
helm repo index docs --url https://cnpst.github.io/charts
```

## Add Helm repository

```
# helm repo add zcp https://cnpst.github.io/charts

# helm repo list
NAME     	URL
...
zcp      	https://cnpst.github.io/charts

# helm search harbor
NAME        	VERSION	DESCRIPTION
zcp/harbor  	0.1.1  	An Enterprise-class Docker Registry by VMware
```

## Deploy Harbor Helm Chart

1. Edit values.yaml

    ```
    # The FQDN for Harbor service.
    externalDomain: harbor.example.com

    # Harbor FQDN as insecure-registries for your docker client.
    insecureRegistry: false

    # The TLS certificate for Harbor. The common name of tlsCrt must match the externalDomain above.
    tlsCrt: |
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----

    tlsKey: |
      -----BEGIN RSA PRIVATE KEY-----
      ...
      -----END RSA PRIVATE KEY-----

    adminserver:
      adminPassword: Harbor12345
    ```

2. Deploy

    ```
    helm install --namespace=harbor --name harbor -f values.yaml zcp/harbor
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
