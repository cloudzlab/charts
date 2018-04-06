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
    TBD
    ```

2. Deploy

```
helm install --namespace=portus --name portus -f values.yaml portus/portus
```
