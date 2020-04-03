# celery-flower

[Flower](https://flower.readthedocs.io/en/latest/) is monitoring tool for use in watching stats and progress within a Python [Celery](http://www.celeryproject.org/) setup.


## Add the Helm Repo

IntrospectData hosts a public helm chart repository at [cloudsmith.io](https://cloudsmith.io). To add our repository, use the followiung command:

```bash
helm repo add id-public https://dl.cloudsmith.io/public/introspect-data/helm-public/helm/charts/
```

## Install Chart

To install the Flower Chart into your Kubernetes cluster :

```bash
helm install --namespace "app_namespace" --name "flower" id-public/celery-flower
```

After installation succeeds, you can get a status of Chart

```bash
helm status "flower"
```

If you want to delete your Chart, use this command:

```bash
helm delete  --purge "flower"
```

### Helm ingresses

The Chart provides ingress configuration to allow customization the installation by adapting
the `values.yaml` depending on your setup.
Please read the comments in the `values.yaml` file for more details on how to configure your reverse
proxy or load balancer.

### Chart Prefix

This Helm automatically prefixes all names using the release name to avoid collisions.

### URL prefix

This chart exposes 1 endpoint:

- Flower Web UI


### Flower configuration

Within the values file for this deployment, 3 values can be set - depending on your backend configuration. These values follow the Celery and Flower standards for addressing the [broker](https://docs.celeryproject.org/en/latest/userguide/configuration.html#broker-settings) and the [results backend](https://docs.celeryproject.org/en/latest/userguide/configuration.html#task-result-backend-settings) within Celery. In addition to the standard Celery configuration, Flower also can take a fully qualified [Broker API](https://flower.readthedocs.io/en/latest/config.html#broker-api) URL for accessing the RabbitMQ Admin API.

```yaml
celery:
  broker:
  broker_api:
  result_backend:
```

## Logs

Logs for Flower are output to `stdout` which can be pulled into any logging setup of your chosing.

## Helm chart Configuration

The following table lists the configurable parameters of the Airflow chart and their default values.

| Parameter                                | Description                                             | Default                   |
|------------------------------------------|---------------------------------------------------------|---------------------------|
| `celery.broker` | Celery broker URI for monitoring access | blank - must be input from existing deployment |
| `celery.broker_api` | RabbitMQ API endpoint in fully qualified format for monitoring access | blank - must be input from existing deployment |
| `celery.result_backend` | Fully qualified URI for accessing the Celery Result Backend | blank - must be input from existing deployment |
| `ingress.enabled`                        | enable ingress                                          | `false`                   |
| `ingress.host`                       | hostname for the webserver ui                           | ""                        |
| `ingress.path`                       | path of the werbserver ui (read `values.yaml`)          | ``                        |
| `ingress.annotations`                | annotations for the web ui ingress                      | `{}`                      |
| `ingress.tls.enabled`                | enables TLS termination at the ingress                  | `false`                   |
| `ingress.tls.secretName`             | name of the secret containing the TLS certificate & key | ``                        |


Full and up-to-date documentation can be found in the comments of the `values.yaml` file.
