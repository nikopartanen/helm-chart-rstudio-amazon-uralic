# Rstudio Helm Chart

This is a test version of RStudio Heml Chart provided by CSC in their [GitHub repository](https://github.com/CSCfi/helm-charts/tree/main/charts/rstudio).

## TL;DR
```sh
helm upgrade --install rstudio .
```

## Explanations
This Helm Chart helps you to deploy Rstudio with Shiny on CSC Rahti 2 (Openshift 4).  
Rstudio is accessible through NGINX. The default user is `rstudio` and the password is generated randomly. You can set you own values by editing the `values.yaml` file and then run:  
```sh
helm upgrade --install rstudio . -f {custom_values.yaml}
```

## Parameters
### Common parameters

| Name                                   | Description                                                   | Value                                                        |
| -------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------ |
| `openshift.enabled`                    | If you are deploying on OKD 4                                 | `true`                                                       |
| `appName`                              | Name for the application                                      | `csc-rstudio-shiny`                                          |
| `buildConfig.gitRepoUri`               | Uri of the GitHub repo where the `Dockerfile` is located      | `https://github.com/CSCfi/helm-charts.git`                   |
| `buildConfig.gitBranch`                | Branch of the GitHub repo                                     | `main`                                                       |
| `buildConfig.contextDir`               | Context directory for the build                               | `charts/rstudio/source`                                      |

### Rstudio parameters

| Name                                           | Description                                                   | Value                             |
| ---------------------------------------------- | ------------------------------------------------------------- | --------------------------------- |
| `rstudio.podSecurityContext`                   | Set SecurityContext for the pod                               | `{}`                              |
| `rstudio.containerSecurityContext`             | Set SecurityContext for the container                         | `allowPrivilegeEscalation: false`<br>`runAsUser:`<br>`runAsGroup:`<br>`capabilities:`<br>&nbsp;&nbsp;`drop:`<br>&nbsp;&nbsp;`- ALL`<br>`runAsNonRoot: true`<br>`seccompProfile:`<br>&nbsp;&nbsp;`type: RuntimeDefault` |
| `rstudio.pvc.mountName`                        | Name for the `PersistentVolumeClaim`                          | `rstudio-server-home`             |
| `rstudio.pvc.storageSize`                      | Storage size for the `PersistentVolumeClaim`                  | `5Gi`                             |
| `rstudio.route.insecureEdgeTerminationPolicy`  | If `openshift.enabled`, will create a route                   | `Redirect`                        |
| `rstudio.route.termination`                    | If `openshift.enabled`, will create a route                   | `edge`                            |
| `rstudio.route.host`                           | If `openshift.enabled`, will create a route. REQUIRED VALUE   | ``                                |
| `rstudio.random_pw_secret_key`                 | Key to store the random password                              | `nginx-password`                  |
| `rstudio.secret.USER`                          | User to connect to Rstudio                                    | `rstudio`                         |
| `rstudio.secret.PASSWORD`                      | Password to connect to Rstudio                                | generated randomly                |

### Shiny parameters

| Name                                         | Description                                                   | Value                             |
| -------------------------------------------- | ------------------------------------------------------------- | --------------------------------- |
| `shiny.pvc.mountName`                        | Name for the `PersistentVolumeClaim`                          | `shiny-server`                    |
| `shiny.pvc.storageSize`                      | Storage size for the `PersistentVolumeClaim`                  | `5Gi`                             |
| `shiny.route.insecureEdgeTerminationPolicy`  | If `openshift.enabled`, will create a route                   | `Redirect`                        |
| `shiny.route.termination`                    | If `openshift.enabled`, will create a route                   | `edge`                            |
| `shiny.route.host`                           | If `openshift.enabled`, will create a route. REQUIRED VALUE   | ``                                |

## Cleanup
To delete all the resources, simply uninstall the Helm Chart:
```sh
helm uninstall rstudio
```

## Links
[Rstudio server configuration](https://docs.posit.co/ide/server-pro/rstudio-server-configuration.html)