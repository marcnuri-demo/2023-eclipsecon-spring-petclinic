# EclipseCon 2023: Helm for Java developers with Eclipse JKube

https://www.eclipsecon.org/2023/sessions/helm-java-developers-eclipse-jkube

## Inner Loop

Deploy application on OpenShift Dev Sandbox

```shell
mvn -DskipTests clean package oc:resource oc:build oc:apply
```

## Outer Loop

Deploy application anywhere

- https://github.com/orgs/marcnuri-demo/packages?repo_name=2023-eclipsecon-spring-petclinic
  - https://github.com/orgs/marcnuri-demo/packages/container/package/2023-eclipsecon-spring-petclinic
  - https://github.com/orgs/marcnuri-demo/packages/container/package/2023-eclipsecon-spring-petclinic-helm
- https://helm.sh/blog/storing-charts-in-oci/
- https://docs.docker.com/docker-hub/oci-artifacts/

Set ghcr.io credentials on `~/.m2/settings.xml`:
```xml
<settings>
  <servers>
    <server>
      <id>ghcr.io</id>
      <username>GitHub-user</username>
      <password>GitHub-Personal-Access-Token</password>
    </server>
  </servers>
</settings>
```

Push Container Image:
```shell
mvn -Pprod -DskipTests clean package k8s:build k8s:push
```

Push Helm chart:
```shell
mvn -Pprod k8s:resource k8s:helm k8s:helm-push
```

Install Helm chart with defaults
```shell
helm install eclipsecon-2023 oci://ghcr.io/marcnuri-demo/2023-eclipsecon-spring-petclinic-helm --version 3.1.0-SNAPSHOT
```

Uninstall Helm chart
```shell
helm uninstall eclipsecon-2023   
```

Install Helm chart overriding values:
```shell
helm install eclipsecon-2023 oci://ghcr.io/marcnuri-demo/2023-eclipsecon-spring-petclinic-helm --version 3.1.0-SNAPSHOT --set ingress.host=customized.dev-sandbox.marcnuri.com
```

