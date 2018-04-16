# Portus

## Helm package

```
git clone https://github.com/cloudzlab/charts.git
cd charts
helm dependency build portus
helm package portus
mv portus-x.x.x.tgz docs
helm repo index docs --url https://cloudzlab.github.io/charts
```

## Add Helm repository

```
# helm repo add zcp https://cloudzlab.github.io/charts

# helm repo list
NAME            URL
...
zcp             https://cloudzlab.github.io/charts

# helm search portus 
NAME            VERSION DESCRIPTION
zcp/portus  	0.1.0  	A Portus Helm chart for Kubernetes. Portus is a...
```

## Configure chart's values

```
  #the internal host names of the portus, registry and nginx service must be covered by the key/cert in order for TLS to work properly
  tls:
    enabled: true
    key: |
      -----BEGIN RSA PRIVATE KEY-----
      ...
      -----END RSA PRIVATE KEY-----

    cert: |
      -----BEGIN CERTIFICATE-----
      ...
      -----END CERTIFICATE-----

  service:
    port: "80"

  host: registry.com

  ingress:
    enabled: true

  tls:
    enabled: true

    # Secrets containing SSL key and cert must be manually created in the namespace
    secretName: "portus-tls"
```

### OAuth(Optional)

1. Gitlab

    ```
    oauth:
      # Callback url: <host>/users/auth/gitlab/callback
      # e.g) If host is registry.com, http://registry.com/users/auth/gitlab/callback
      gitlab:
        enabled: true
        applicationId: xxx
        secret: xxx
        group: xxx
        #domain:
        #server:    
    ```
2. Open ID

    ```
    # OpenID authentication support. If enabled, then users can authenticate with OpenID/Connect
    # Callback url: <host>/users/auth/open_id/callback
    open_id:
      enabled: false
      # Optional. If identifier set then user redirect to the OpenID provider.
      # If not, then user is asked for identifier before redirect.
      # Example https://openid.stackexchange.com
      identifier: ""
      # If a domain (e.g. mycompany.com) is set, then only signups with email from this domain are allowed.
      domain: ""
    ```
    
## Deploy

    ```
    helm install --namespace=portus --name portus -f values.yaml zcp/portus
    ```
