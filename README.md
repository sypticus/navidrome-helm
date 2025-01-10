# Navidrome Helm

This is a helm chart for [Navidrome](https://github.com/navidrome/navidrome), allowing you to easily deploy the excellent music manager to your Kubernetes cluster.


https://github.com/sypticus/navidrome-helm.git

## Installing the Chart.

```console
git clone https://github.com/sypticus/navidrome-helm.git
`helm install navidrome ./navidrome -n navidrome --create-namespace`
```

You should then be able to access the UI.

```console
POD=$(kubectl get pods -l app.kubernetes.io/instance=navidrome  -o jsonpath="{.items[0].metadata.name}" -n navidrome)
kubectl -n proxy port-forward $POD 4533
```
In a browser window open http://localhost:4533

## Uninstalling the Chart

```console
helm delete jacket
```


## Persistence

Until you set up persistence any changes you make will be dropped when the pod restarts, and you won't have anywhere you store your music.

Turn on `persistence.enabled` to allow storage.

If you already have PVCs you would like to use for data and music storage, set them in `values.yaml`, 
otherwise a two new PVCs will be created for you using the default storage class.

```console
helm install navidrome ./navidrome-helm/navidrome -n navidrome --create-namespace --set persistence.enabled=true
```



## Deploying for Real

In order to set the correct values for your instance, create a values.yaml to override the defaults. 

```console
cp ./navidrome-helm/navidrome/values.yaml ./my_values.yaml

#Edit that file and set your custom values

helm install navidrome ./navidrome-helm/navidrome -f my_values.yaml -n navidrome
```


## Configuration

Documentation of configs can be found on the Navidrome [documentation](https://www.navidrome.org/docs/usage/configuration-options/)
Further info can be found in the charts included [values.yaml](https://github.com/sypticus/navidrome-helm/blob/main/navidrome/values.yaml)


The following are the basic config values which can be set in the helm chart.
All other advanced configuration values can be passed in as environmental variables using the `advancedConfiguration` field

| Parameter                              | Description                                                                                                                 | Default            |
|----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|--------------------|
| `image.repository`                     | Image repository                                                                                                            | `deluan/navidrome` |
| `image.tag`                            | Image tag                                                                                                                   | `latest`           |
| `image.pullPolicy`                     | Image pull policy                                                                                                           | `IfNotPresent`     |
| `musicFolder`                          | Path for music folder                                                                                                       | `/music`           |
| `dataFolder`                           | Path for data storage folder                                                                                                | `/appData`         |
| `cacheData`                            | Path for cache data folder                                                                                                  | `/cache/`          |
| `service.type`                         | Kubernetes service type                                                                                                     | `ClusterIP`        |
| `service.port`                         | Port to be used for http calls.                                                                                             | `4533`             |
| `service.loadBalancerIP`               | If type is LoadBalancer, set the IP                                                                                         |                    |
| `persistence.enabled`                  | Use a PVC to save data and music                                                                                            | `false`            |
| `persistence.appData.existingClaim`    | An existing VPC to use for app data storage                                                                                 |                    |
| `persistence.appData.storageClassName` | Storage class to use if creating a new app data PVC                                                                         |                    |
| `persistence.appData.pvcSize`          | Size of new app data PVC                                                                                                    | `10G`              |
| `persistence.music.existingClaim`      | An existing VPC to use for app data storage                                                                                 |                    |
| `persistence.music.storageClassName`   | Storage class to use if creating a new music PVC                                                                            |                    |
| `persistence.music.pvcSize`            | Size of new music PVC                                                                                                       | `30G`              |
| `advancedConfiguration`                | List of environment variables to further [configure](https://www.navidrome.org/docs/usage/configuration-options/) Navidrome |                    |
| `enableInsightsCollector`              | Allow Anonymous Data Collection by Navidrome to help improve the project                                                    | `true`             |


