# Traces with Tempo

Set up and configure [Tempo](https://grafana.com/oss/tempo/) with Alloy and Grafana to look for traces in your Kubernetes cluster.

## Charts installation

Before installing all the components, don't forget to check default configurations and update some values according to your needs!

Create a kind cluster or use your own Kubernetes cluster:

```sh
kind create cluster --config ./kind.yaml --name k8s-tempo
```

Define helm repositories for charts installation:

```sh
helm repo add grafana https://grafana.github.io/helm-charts
helm repo add traefik https://traefik.github.io/charts # For demo purposes
helm repo update
```

Set up Tempo:

```sh
helm install tempo grafana/tempo-distributed --version 1.32.7 --namespace observability --create-namespace --values ./values-tempo.yaml
```

Install Alloy:

```sh
helm install alloy grafana/alloy --version 0.12.5 --namespace observability --create-namespace --values ./values-alloy.yaml
```

Configure and deploy Grafana:

```sh
helm install grafana grafana/grafana --version 8.10.4 --namespace observability --create-namespace --values ./values-grafana.yaml
```

And finally, you can use Traefik to inject a few traces into Tempo as a demo:

```sh
helm install traefik traefik/traefik --version 34.4.1 --namespace ingress --create-namespace --values ./values-traefik.yaml
```

Get access to Grafana with the ``kubectl port-forward`` command if there is no ingress configured:

```sh
kubectl -n observability port-forward svc/grafana 8080:80
```

## Blog posts

Don't hesitate to read the following blog posts to find out more about configuring Tempo:

* [Track your traces with Tempo](https://blog.filador.fr/en/posts/track-your-traces-with-tempo) - English version
* [À la recherche de vos traces avec Tempo](https://blog.filador.fr/posts/a-la-recherche-de-vos-traces-avec-tempo/) - French version
