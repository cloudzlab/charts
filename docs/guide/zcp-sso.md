# ZCP SSO

## Helm package

```
git clone https://github.com/cnpst/zcp-sso.git
helm package zcp-sso

git clone https://github.com/cnpst/charts
mv zcp-sso-x.x.x.tgz charts/docs
helm repo index charts/docs --url https://cnpst.github.io/charts
```

## Add Helm repository

```
# helm repo add zcp https://cnpst.github.io/charts

# helm repo list
NAME     	URL
...
zcp      	https://cnpst.github.io/charts

# helm search zcp-sso
NAME        	    VERSION	 DESCRIPTION
zcp/zcp-sso     	x.x.x  	 xxx
```

## Deploy ZCP SSO Helm Chart

1. Create values.yaml

    ```
    ingress:
      enabled: true
      annotations: {}
      path: /
      hosts:
        - sso.example.com
      tls:
        - secretName: example-tls
          hosts:
            - sso.example.com

    configmap:
      targetUrl: "http://url-to-the-target-server.example.com"
      realm: REALM_NAME
      realmPublicKey: "REALM_PUBLIC_KEY"
      authServerUrl: http://url-to-keycloak.example.com/auth
      resource: CLIENT_ID
      secret: CLIENT_SECRET
      pattern: /admin
      rolesAllowed: admin
    ```

2. Create secret

    ```
    $ kubectl create secret tls example-tls --cert example.crt --key example.key
    ```

2. Deploy

    ```
    helm install --name zcp-sso -f values.yaml zcp/zcp-sso
    ```

3. Optional

    - To change POD's resource

    ```
    resources:
      limits:
      cpu: 100m
      memory: 128Mi
      requests:
        cpu: 100m
        memory: 128Mi
    ```
