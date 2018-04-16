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

## Deploy Portus Helm Chart

1. Edit values.yaml

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

2. Deploy

    ```
    helm install --namespace=portus --name portus -f values.yaml zcp/portus
    ```
