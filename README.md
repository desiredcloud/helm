# Starting with Helm 


## Helm Repositories

- Add helm repo on your local machine:
  ```yaml
  helm repo add [NAME] [URL]
  helm repo add crossplane-stable https://charts.crossplane.io/stable
  ```

- List already added repositories:
  ```yaml
    helm repo list
    NAME             	URL                                      
    tremolo          	https://nexus.tremolo.io/repository/helm/
    bitnami          	https://charts.bitnami.com/bitnami       
    kasten           	https://charts.kasten.io/                
    hashicorp        	https://helm.releases.hashicorp.com      
    crossplane-stable	https://charts.crossplane.io/stable 
  ```
  
- Update existing added repositories:
  ```yaml
   helm repo update
    Hang tight while we grab the latest from your chart repositories...
    ...Successfully got an update from the "hashicorp" chart repository
    ...Successfully got an update from the "crossplane-stable" chart repository
    ...Successfully got an update from the "kasten" chart repository
    ...Successfully got an update from the "bitnami" chart repository
    ...Successfully got an update from the "tremolo" chart repository
    Update Complete. ⎈Happy Helming!⎈
  ```
  
- Remove existing repositories:
  ```yaml
  helm repo remove tremolo
  "tremolo" has been removed from your repositories
  ```

---
## Helm Plugins

A [Helm plugin](https://helm.sh/docs/topics/plugins/) is a tool that can be accessed through the helm CLI, but which is not part of the built-in Helm codebase.

- Helm [Plugins](https://github.com/topics/helm-plugins) 

Examples:

- `Helm Diff`: Performs a diff between a deployed release and proposed Helm upgrade
- `Helm Secrets`: Used to help conceal secrets from Helm charts
- `Helm Monitor`: Used to monitor a release and perform a rollback if certain events occur
- `Helm Unittest`: Used to perform unit testing on a Helm chart

---
## Helm ENV Vars

```bash
helm -h
The Kubernetes package manager

Environment variables:

| Name                               | Description                                                                                       |
|------------------------------------|---------------------------------------------------------------------------------------------------|
| $HELM_CACHE_HOME                   | set an alternative location for storing cached files.                                             |
| $HELM_CONFIG_HOME                  | set an alternative location for storing Helm configuration.                                       |
| $HELM_DATA_HOME                    | set an alternative location for storing Helm data.                                                |
| $HELM_DEBUG                        | indicate whether or not Helm is running in Debug mode              
```

---

| Operating System | Cache Path                        | Configuration Path               | Data Path                        |
|------------------|----------------------------------|----------------------------------|----------------------------------|
| Windows          | `%TEMP%\helm`                     | `%APPDATA%\helm`                 | `%APPDATA%\helm`                 |
| macOS            | `$HOME/Library/Caches/helm`       | `$HOME/Library/Preferences/helm` | `$HOME/Library/helm`             |
| Linux            | `$HOME/.cache/helm`               | `$HOME/.config/helm`             | `$HOME/.local/share/helm`        |

- Helm uses the cache path to store charts that are downloaded from upstream chart repositories. Installed charts are cached to the local machine to enable faster installation of the chart the next time it is referenced. The cache path also includes YAML files that are used to index the available Helm charts from each configured repository. These index files are updated when users run the `helm repo update` command.

- The configuration path is used to save repository information, such as the URL and credentials for authentication, if required. When a chart is installed but is not located in the local cache yet, Helm uses the configuration path to look up the URL of the chart repository. The chart is then downloaded from this URL.

- The data path is used to store plugins. When a plugin is installed using the helm plugin install command, the plugin itself is stored in this location.

---

## [TAB Completion](https://helm.sh/docs/helm/helm_completion/)

- For bash
```shell
source <(helm completion bash)
```

- For zsh
```shell
source <(helm completion zsh)
```

---

## [Artifact Hub](https://www.cncf.io/projects/artifact-hub/)

- Find, install and publish Kubernetes packages

- Will search `wordpress` chart in ArtifactHub
```bash
helm search hub wordpress --list-repo-url

helm search hub wordpress --output yaml
```

- Will add the `Bitnami` repo in your local index.
  - `archive-full-index` will list all the old charts as well.
```shell
% helm repo add bitnami https://raw.githubusercontent.com/bitnami/charts/archive-full-index/bitnami 
% helm repo list
NAME             	URL                                                                                                              
bitnami          	https://raw.githubusercontent.com/bitnami/charts/archive-full-index/bitnami
```

- Will list all the available charts under `binami` repo.
  - Always run `helm repo update`, this will update you locally added repos.
```shell
helm search repo bitnami
NAME                                        	CHART VERSION	APP VERSION  	DESCRIPTION                                       
bitnami/bitnami-common                      	0.0.9        	0.0.9        	DEPRECATED Chart with custom templates used in ...
bitnami/airflow                             	16.1.8       	2.8.0        	Apache Airflow is a tool to express and execute...
bitnami/apache                              	10.2.4       	2.4.58       	Apache HTTP Server is an open-source HTTP serve...
...

helm search repo bitnami --output yaml

helm search repo wordpress           
NAME                   	CHART VERSION	APP VERSION	DESCRIPTION                                       
bitnami/wordpress      	19.0.4       	6.4.2      	WordPress is the world's most popular blogging ...
bitnami/wordpress-intel	2.1.31       	6.1.1      	DEPRECATED WordPress for Intel is the most popu...
```

- Search all previous versions

```shell
helm search repo wordpress --versions
NAME                   	CHART VERSION	APP VERSION   	DESCRIPTION                                       
bitnami/wordpress      	19.0.4       	6.4.2         	WordPress is the world's most popular blogging ...
bitnami/wordpress      	19.0.3       	6.4.2         	WordPress is the world's most popular blogging ...
bitnami/wordpress      	19.0.2       	6.4.2         	WordPress is the world's most popular blogging ...
...
```

- Showing all information of the chart:
```shell
helm show chart bitnami/wordpress 
# OR
# helm show chart bitnami/wordpress --version 19.0.4

annotations:
  category: CMS
  images: |
    - name: wordpress
      image: docker.io/bitnami/wordpress:6.4.2-debian-11-r10
  licenses: Apache-2.0
apiVersion: v2
appVersion: 6.4.2
dependencies:
...
description: WordPress ...
home: https://bitnami.com
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- application
name: wordpress
sources:
- https://github.com/bitnami/charts/tree/main/bitnami/wordpress
version: 19.0.4
```

```shell
Usage:
  helm show [command]

Available Commands:
  all         show all information of the chart
  chart       show the chart's definition
  crds        show the chart's CRDs
  readme      show the chart's README
  values      show the chart's values
```

---

- To see chart's default values
```shell
helm show values bitnami/wordpress --version 12.1.6
```

- Overriding values, using `values.yaml`
```shell
cat wordpress-values.yaml 
service:
  type: NodePort
wordpressUsername: helm-user
wordpressPassword: my-password
wordpressEmail: helm-user@example.com
wordpressFirstName: Helm_is
wordpressLastName: Fun
wordpressBlogName: Learn Helm!
```

- Installing helm chart
```shell
helm install [NAME] [CHART] [flags]
```

```shell
kubectl create namespace wordpress
```

```shell
helm install wordpress bitnami/wordpress --values=values.yaml -n wordpress --version 12.1.6
NAME: wordpress
LAST DEPLOYED: Wed Feb  7 20:55:36 2024
NAMESPACE: wordpress
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
```

- Get notes of a release
- First, get list of releases
```shell
helm list -A
NAME     	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART           	APP VERSION
wordpress	wordpress	1       	2024-02-07 20:55:36.291079 +0530 IST	deployed	wordpress-12.1.6	5.8.0      
```
- Get notes by release name
```shell
helm get notes wordpress -n wordpress
NOTES:
Your WordPress site can be accessed through the following DNS name from within your cluster:
..
```

- Get manifest of named release
```shell
helm get manifest wordpress -n wordpress
```

- You can get following details around any release, using `helm get` command.
```shell
helm get -h                             

Available Commands:
  all         download all information for a named release
  hooks       download all hooks for a named release
  manifest    download the manifest for a named release
  notes       download the notes for a named release
  values      download the values file for a named release
```

---

- Get values that has been applied on the installed release:
```shell
helm get values wordpress -n wordpress
```

- - Get all values supported by this release :
```shell
helm get values wordpress --all -n wordpress
```

- Many Helm charts add the app.kubernetes.io/name label (or a similar label) on the resources they create.
```shell
kubectl get all -l app.kubernetes.io/name=wordpress -n wordpress
```

---

## Upgrading WordPress Release

- Let's update `values.yaml` and add following values:
  ```shell
  replicaCount: 2
  resources:
    requests:
      memory: 256Mi
      cpu: 100m
  ```

```shell
helm upgrade [RELEASE] [CHART] [flags]

helm upgrade --install wordpress bitnami/wordpress --values values.yaml -n wordpress --version 12.1.6
```
```shell
helm list -n wordpress
NAME     	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART           	APP VERSION
wordpress	wordpress	2       	2024-02-12 08:43:52.103338 +0530 IST	deployed	wordpress-12.1.6	5.8.0      
```

- Helm upgrade command includes two additional values-related flags.
  - `--reuse-values` (default): When upgrading, reuse the last release’s values
  - `--reset-values`: When upgrading, reset the values to the chart defaults
  - If at least one value is provided with `--set` or `--values`, then the `--reset-values` flag is applied by default.

---

## Rolling back 

- Every Helm release has a history of `revisions`.
- Revision data is saved in Kubernetes Secrets by default.

```shell
kubectl get secrets -n wordpress | grep release
sh.helm.release.v1.wordpress.v1   helm.sh/release.v1   1      4d12h
sh.helm.release.v1.wordpress.v2   helm.sh/release.v1   1      39m
```

- Each of these Secrets corresponds with an entry of the release’s revision history

```shell
helm history wordpress -n wordpress
REVISION	UPDATED                 	STATUS    	CHART           	APP VERSION	DESCRIPTION     
1       	Wed Feb  7 20:55:36 2024	superseded	wordpress-12.1.6	5.8.0      	Install complete
2       	Mon Feb 12 08:43:52 2024	deployed  	wordpress-12.1.6	5.8.0      	Upgrade complete
```

- We can check the values, that has been applied on a specific `REVISION`.
```shell
helm get values wordpress --revision 2 -n wordpress 
USER-SUPPLIED VALUES:
...
```

`helm rollback <RELEASE> [REVISION] [flags]`

```shell
helm history wordpress -n wordpress
REVISION	UPDATED                 	STATUS    	CHART           	APP VERSION	DESCRIPTION     
1       	Wed Feb  7 20:55:36 2024	superseded	wordpress-12.1.6	5.8.0      	Install complete
2       	Mon Feb 12 08:43:52 2024	deployed  	wordpress-12.1.6	5.8.0      	Upgrade complete

helm rollback wordpress 1 -n wordpress
Rollback was a success! Happy Helming!

helm history wordpress -n wordpress   
REVISION	UPDATED                 	STATUS    	CHART           	APP VERSION	DESCRIPTION     
1       	Wed Feb  7 20:55:36 2024	superseded	wordpress-12.1.6	5.8.0      	Install complete
2       	Mon Feb 12 08:43:52 2024	superseded	wordpress-12.1.6	5.8.0      	Upgrade complete
3       	Mon Feb 12 09:44:30 2024	deployed  	wordpress-12.1.6	5.8.0      	Rollback to 1   
```

---

## Uninstalling helm Release

```shell
helm uninstall RELEASE_NAME [...] [flags]
```

```shell
helm uninstall wordpress -n wordpress
release "wordpress" uninstalled
```

- Helm will not delete any additional resources created by the resources from the helm chart.
- For example, a PVC got created for the statefulset. Which is not part of chart.
- So, will have to manually delete that.
```shell
kubectl get pvc -n wordpress
NAME                       STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-wordpress-mariadb-0   Bound    pvc-42962f89-6595-4982-bf3d-6d5839babf73   8Gi        RWO            standard       4d13h

kubectl delete pvc data-wordpress-mariadb-0 -n wordpress
persistentvolumeclaim "data-wordpress-mariadb-0" deleted
```
















Here's the data formatted as a Markdown table:

| File/Directory          | Definition                                                                                           | Required?                            |
|-------------------------|------------------------------------------------------------------------------------------------------|--------------------------------------|
| charts/                 | A directory that contains dependencies or Helm charts that the parent chart depends on.              | No                                   |
| Chart.yaml              | A file that contains metadata about the Helm chart.                                                   | Yes                                  |
| .helmignore             | A file that contains a list of files and directories that should be omitted from the Helm chart’s packaging. | No                                   |
| templates/              | A directory that contains Golang templates, which are primarily used for generating Kubernetes resources. | Yes, unless the chart contains dependencies |
| templates/*.yaml        | A template file used to generate a Kubernetes resource.                                               | Yes, unless the chart contains dependencies |
| templates/_*.tpl        | A file that contains boilerplate helper templates.                                                    | No                                   |
| templates/NOTES.txt    | A template file that is used to generate usage instructions after chart installation.                 | No                                   |
| templates/tests/        | A folder used for grouping different templates. This is strictly for aesthetics and has no effect on how the Helm chart operates – for example, templates/tests is used to group templates that are used for testing. | No                                   |
| values.yaml             | A file that contains the chart’s default values.                                                      | No, but every chart should contain this file as a best practice |