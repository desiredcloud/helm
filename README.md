# helm


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
#### Helm Plugins

A [Helm plugin](https://helm.sh/docs/topics/plugins/) is a tool that can be accessed through the helm CLI, but which is not part of the built-in Helm codebase.

- Helm [Plugins](https://github.com/topics/helm-plugins) 

Examples:

- `Helm Diff`: Performs a diff between a deployed release and proposed Helm upgrade
- `Helm Secrets`: Used to help conceal secrets from Helm charts
- `Helm Monitor`: Used to monitor a release and perform a rollback if certain events occur
- `Helm Unittest`: Used to perform unit testing on a Helm chart

---
 #### Helm ENV Vars

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

- Helm uses the cache path to store charts that are downloaded from upstream chart repositories. Installed charts are cached to the local machine to enable faster installation of the chart the next time it is referenced. The cache path also includes YAML files that are used to index the available Helm charts from each configured repository. These index files are updated when users run the helm repo update command.

- The configuration path is used to save repository information, such as the URL and credentials for authentication, if required. When a chart is installed but is not located in the local cache yet, Helm uses the configuration path to look up the URL of the chart repository. The chart is then downloaded from this URL.

- The data path is used to store plugins. When a plugin is installed using the helm plugin install command, the plugin itself is stored in this location.

---

#### [TAB Completion](https://helm.sh/docs/helm/helm_completion/)

- For bash
```shell
source <(helm completion bash)
```

- For zsh
```shell
source <(helm completion zsh)
```

---

#### [Artifact Hub](https://www.cncf.io/projects/artifact-hub/)

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
Available Commands:
  all         show all information of the chart
  chart       show the chart's definition
  crds        show the chart's CRDs
  readme      show the chart's README
  values      show the chart's values
```

---

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