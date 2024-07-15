Blocker
Jenkins not executing jobs (build executor offline, pending - waiting for next executor)

Solution
go to Jenkins -> Manage Jenkins -> Nodes -> Built-In Node -> Configure

Examine the "Number of executors". (Click on configure icon)

The number of executors was set to 0. Increased it and issue got fixed.

You should also explore the JCasC section and see all the configured credentials and pipelines.

You can now create more secrets to connect to other tools, such as Sonarqube, AWS, KUBECONFIG etc…

Now, attempt to configure a remote artifactory repository.

1. Create a remote repository in Artifactory, so that instead of pulling helm charts directly from https://charts.jenkins.io for example, you will pull directly from your configured remote repository in Artifactory. This helps organisations stay secure. Artifactroy will therefore be a proxy to pulling from the internet.

2. Configure remote repository for the following chart repos, search the repos and try to install tools from them through the Artifactroy remote repository you have configured.

```
hashicorp               https://helm.releases.hashicorp.com
jenkins                 https://charts.jenkins.io
gitlab                  https://charts.gitlab.io
eks                     https://aws.github.io/eks-charts
jfrog                   https://charts.jfrog.io
prometheus-community    https://prometheus-community.github.io/helm-charts
kube-state-metrics      https://kubernetes.github.io/kube-state-metrics
grafana                 https://grafana.github.io/helm-charts
ingress-nginx           https://kubernetes.github.io/ingress-nginx
codecentric             https://codecentric.github.io/helm-charts
openstack-helm          https://registry.thecore.net.au/chartrepo/openstack
gabibbo97               https://gabibbo97.github.io/charts/
chartmuseum             https://chartmuseum.github.io/charts
kubernetes-dashboard    https://kubernetes.github.io/dashboard/
jenkinsci               https://charts.jenkins.io/
oauth2-proxy            https://oauth2-proxy.github.io/manifests
cert-manager            https://charts.jetstack.io
nginx                   https://helm.nginx.com/stable
sonarqube               https://SonarSource.github.io/helm-chart-sonarqube
istio                   https://istio-release.storage.googleapis.com/charts
```

For upgrading and troubleshooting

```
helm upgrade -i ingress-nginx ingress-nginx/ingress-nginx --set controller.service.type=ClusterIP -n ingress-nginx

helm upgrade -i ingress-nginx ingress-nginx/ingress-nginx --set controller.service.type=LoadBalancer -n ingress-nginx

helm upgrade -i ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx
```