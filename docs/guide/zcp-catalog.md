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

1. Create values.yaml

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
        secretName: example-tls

    mongodb:
      persistence:
        enabled: false
    ```
    
2. Create secret

    ```
    $ kubectl create secret tls example-tls --cert example.crt --key example.key
    ```

3. Deploy

    ```
    helm install --name zcp-catalog -f values.yaml zcp/zcp-catalog
    ```

4. Optional

    - To use zcp sso
    
        Deploy zcp sso helm chart. User who has "admin" role is able to access zcp-catalog ui with below config.<br>
        Refer to [ZCP SSO Helm Chart](https://github.com/cnpst/zcp-sso) for more information.
        
        ```
        helm install --name zcp-sso-for-catalog -f values.yaml zcp/zcp-sso

        <values.yaml>
        configmap:
          targetUrl: "http://zcp-catalog-zcp-catalog-ui"
          realm: REALM_NAME
          realmPublicKey: "REALM_PUBLIC_KEY"
          authServerUrl: http://url-to-keycloak.example.com/auth
          resource: CLIENT_ID
          secret: CLIENT_SECRET
          pattern: /*
          rolesAllowed: admin
        ```
    
        Edit zcp-catalog ingress.
    
        ```
        $ kubectl edit zcp-catalog-zcp-catalog
        ...
        spec:
          rules:
          - host: catalog.example.com
            http:
              paths:
              - backend:
                  serviceName: zcp-sso-for-catalog
                  servicePort: 80
                path: /
              - backend:
                  serviceName: zcp-catalog-zcp-catalog-ui
                  servicePort: 80
                path: /ui
              - backend:
                  serviceName: zcp-catalog-zcp-catalog-api
                  servicePort: 80
                path: /api/
          tls:
          - hosts:
            - catalog.example.com
            secretName: example-tls
        ```
    
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
