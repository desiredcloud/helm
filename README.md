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

