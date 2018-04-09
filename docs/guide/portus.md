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

## Deploy Portus Helm Chart

1. Edit values.yaml

    Use NodePort 

    ```
      service:
      type: "NodePort"

      # Docker registry url(External)
      host: registry.com
    ```

    Use Ingress

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
        type: "ClusterIP"
        port: "443"
        
      host: registry.com
      
      ingress:
        enabled: true
      
      annotations:
        ingress.kubernetes.io/rewrite-target: /
        
      tls:
        enabled: true
        
        # Secrets containing SSL key and cert must be manually created in the namespace
        secretName: "portus-tls"
    ```

2. Deploy

    ```
    helm install --namespace=portus --name portus -f values.yaml portus/portus
    ```
