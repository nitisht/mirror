# mirror

Helm Chart to enable mc mirror across two MinIO clusters.

## Prerequisite

Setup local chart repository to serve this chart using [Chartmuseum](https://github.com/helm/chartmuseum). Download `chartmuseum` as [explained here](https://github.com/helm/chartmuseum#installation). Start the `chartmuseum` server like so:

```sh
chartmuseum --debug --port=8080   --storage="local"   --storage-local-rootdir="./chartstorage"
```

Then clone this repository locally and navigate to the directory. Then use the below commands:

```sh
helm package ./
curl --data-binary "@mirror-0.1.0.tgz" http://localhost:8080/api/charts
```

Then add helm `chartmuseum` repo using the below command:

```sh
helm repo add chartmuseum http://localhost:8080
helm repo update
```

It is expected that you have a Kubernetes cluster running at this point, with `helm` configured to talk to the cluster.

## Get Started

Now that everything is configured, lets start using the Chart. Use the fields `env.MC_HOST_<alias>` to set alias for MinIO servers. By default there are two aliases configured `play` and `play1`, both pointing to MinIO Play server at `https://play.min.io`. Set alias, copy source and target via command line like so:

```sh
helm install chartmuseum/mirror --set env.MC_HOST_myminio=https://Q3AM3UQ867SPQQA43P2F:zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG@play.min.io,location.source=myminio/source1,location.target=myminio/target1 --generate-name
```

Confirm if the `mc mirror` is running successfully using:

```sh
kubectl get pods
```

Finally verify if data is being mirrored from Source to target using `mc ls` or MinIO browser.