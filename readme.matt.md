This is a fork of https://github.com/wiz-sec/charts/# to test support for two wiz-api-tokens in the wiz-kubernetes-integration chart.

Changes to original:
- modified:   wiz-kubernetes-integration/templates/secrets-wiz-api-token.yaml
- modified:   wiz-kubernetes-integration/values.yaml

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

Remove:
```sh
helm delete wiz-kubernetes-integration -n wiz
```


# Helm Dependency Files
These are Helm dependency management artifacts:

Chart.lock

- Lock file that records the exact versions of dependencies that were resolved
- Similar to package-lock.json in npm or Gemfile.lock in Ruby
- Ensures reproducible builds by locking specific versions
- Contains SHA digest to verify integrity

charts/ directory

- Downloaded dependency charts as compressed .tgz files
- These are the actual subchart packages downloaded from https://wiz-sec.github.io/charts
- Created when we ran helm dependency build to test the templates

Should You Keep Them?

For version control:
- Chart.lock: Usually committed to ensure reproducible deployments
- charts/: Usually ignored (added to .gitignore) since they can be regenerated

For deployment:
- Both are needed when installing the chart
- helm dependency build regenerates them from Chart.yaml

Current Status

These were created because we needed to test the template rendering. In a production environment, you'd typically:

1. Add charts/ to .gitignore
2. Commit Chart.lock
3. Let CI/CD or deployment process run helm dependency build as needed