# Helm

## Useful links

* [https://helm.sh/docs/topics/version_skew/](compatibility matrix)
* [https://helm.sh/docs/intro/cheatsheet/](cheat sheet)
* [https://artifacthub.io/](Artifact Hub,)

## Commands

```bash
helm repo list  # list chart repositories
helm repo add bitnami https://charts.bitnami.com/bitnami  # add a chart repository
helm repo remove bitnami
helm repo update  # make get the latest list of charts

helm search hub  # searches the Artifact Hub, which lists helm charts from dozens of different repositories
helm search hub vault
helm search repo  # searches the repositories that you have added to your local helm client
helm search repo bitnami  # list the charts you can install within specified repo
helm search repo bitnami --regexp *grafana*  # FIXME: provide correct example

helm show values bitnami/mysql  # show what options are configurable on a chart
helm show all bitnami/mysql  # inspect a chart and list its contents

helm pull bitnami/wordpress  # download a chart as a .tgz file

helm install my-mysql bitnami/mysql  # or
helm install bitnami/mysql --generate-name
helm install --debug --dry-run my-mysql bitnami/mysql

helm upgrade my-mysql oci://registry-1.docker.io/bitnamicharts/mysql --set volumePermissions.enabled=true  # fix Permission denied issue https://docs.bitnami.com/general/how-to/troubleshoot-helm-chart-issues/

helm get values my-mysql
helm get values --revision=1 my-mysql
helm get manifest my-mysql  # prints out all of the Kubernetes resources that were uploaded to the server within specified release

helm list
helm list --uninstalled  # only show releases that were uninstalled with the --keep-history flag
helm list --all  #  show you all release records that Helm has retained, including records for failed or deleted items

helm status my-mysql
helm history my-mysql

helm uninstall my-mysql --keep-history

helm rollback my-mysql
helm rollback my-mysql 2  # rollback to specific previous version

helm create my-chart  # now there is a chart in ./my-chart. You can edit it and create your own templates.
helm package my-chart
helm lint my-chart
helm install my-chart ./my-chart-0.1.0.tgz
```

## Questions

## What is Helm?

Helm is the Kubernetes package manager.

Helm installs charts into Kubernetes, creating a new release for each installation. And to find new charts, you can search Helm chart repositories.

## What is Chart?

A Chart is a Helm package.
A Chart is a collection of files that describe a related set of Kubernetes resources.

It contains all of the resource definitions necessary to run an application, tool, or service inside of a Kubernetes cluster.

## What is Repository?

A chart repository is an HTTP server that houses one or more packaged charts.

A Repository is the place where charts can be collected and shared.

## What is Release?

A Release is an instance of a chart running in a Kubernetes cluster.

One chart can often be installed many times into the same cluster. And each time it is installed, a new release is created.

## What does "install a new package" in the context of Helm mean?

Installing a chart creates a new release object.

## In which order does Helm install resources?

[https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package](here is the answer)

Note: Helm does not wait until all of the resources are running before it exits.

## What are the possible sources from which the "helm install" command can install?

* chart repository          - `helm install foo for-repo/foo`
* local chart archive       - `helm install foo foo-0.1.1.tgz`
* unpacked chart directory  - `helm install foo path/to/foo`
* full URL                  - `helm install foo https://example.com/charts/foo-1.2.3.tgz`

## What are global variables?

Values files support global variables.

This provides a way of sharing one top-level variable with all subcharts, which is useful for things like setting metadata properties like labels.

They can be defined with the help of the `global` section

The global values can be accessed as `{{ .Values.global.app }}` in any chart (parent or child).

## How does Helm work with CRDs?

CRD YAML files should be placed in the `crds/` directory inside of a chart.

CRD files cannot be templated. They must be plain YAML documents.

CRD information is available in the `.Capabilities` object in Helm templates.

## What is Starter Chart?

Starter Chart is a template or a blueprint for creating new Helm charts. It provides a basic structure that you can use to kickstart the development of a new chart, offering default files and directories that conform to best practices.

Starter Charts are meant to provide a quick and standardized way to create new Helm charts with a pre-defined structure.

## What is Hook?

Helm provides a hook mechanism to allow chart developers to intervene at certain points in a release's life cycle.

The hooks are

* pre-install
* post-install
* pre-delete
* post-delete
* pre-upgrade
* post-upgrade
* pre-rollback
* post-rollback
* test

## What are the top-level objects that you can access in templates?

* Release - describes the release itself
* Values - values passed into the template from the `values.yaml` file and from user-supplied files
* Chart - content of `Chart.yaml`
* Subcharts - provides access to the scope of subcharts
* Files
* Capabilities
* Template

## What is template directive?

A template directive is enclosed in `{{` and `}}` blocks.

The template directive `{{ .Release.Name }}` injects the release name into the template.
