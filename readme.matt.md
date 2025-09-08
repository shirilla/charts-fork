This is a fork of https://github.com/wiz-sec/charts/# to test support for two wiz-api-tokens in the wiz-kubernetes-integration chart.

Changes to original:
modified:   wiz-kubernetes-integration/templates/secrets-wiz-api-token.yaml
modified:   wiz-kubernetes-integration/values.yaml

```sh
git clone https://github.com/shirilla/charts-fork.git
cd charts-fork

k create ns wiz

kubectl -n wiz create secret generic wiz-api-token \
  --from-literal clientId=your-client-id-1 \
  --from-literal clientToken=your-client-secret-1


kubectl -n wiz create secret generic wiz-api-token-2 \
  --from-literal clientId=your-client-id-2 \
  --from-literal clientToken=your-client-secret-2

 kubectl -n wiz create secret docker-registry sensor-image-pull   --docker-server=docker-server   --docker-username=docker-user   --docker-password=docker-password

helm upgrade wiz-kubernetes-integration ./wiz-kubernetes-integration -f ./values-two-api-token.yaml --create-namespace -n wiz --install
```

