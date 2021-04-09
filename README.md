tika-helm
=========

<!--[![Build Status](https://img.shields.io/jenkins/s/https/devops-ci.elastic.co/job/elastic+helm-charts+master.svg)](https://devops-ci.elastic.co/job/elastic+helm-charts+master/)-->
[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/apache)](https://artifacthub.io/packages/search?repo=apache)

A [Helm chart][] to deploy [Apache Tika][] on [Kubernetes][].

<img src="https://tika.apache.org/tika.png" width="300" />

This Helm chart is a lightweight way to configure and run the official [Tika Docker image][].

We recommend that the Helm chart version is aligned to the version Tika you want to deploy. 
This will ensure that you using a chart version that has been tested against the corresponding 
production version. This will also ensure that the documentation and examples for the chart 
will work with the version of Tika you are installing.

<!-- development warning placeholder -->
**Warning**: This branch is used for development, please use the [latest release][] for released version.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Requirements](#requirements)
- [Installing](#installing)
  - [Install released version using Helm repository](#install-released-version-using-helm-repository)
  - [Install development version using master branch](#install-development-version-using-master-branch)
- [Upgrading](#upgrading)
- [Usage notes](#usage-notes)
- [Configuration](#configuration)
  - [Deprecated](#deprecated)
- [FAQ](#faq)
- [Contributing](#contributing)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- Use this to update TOC: -->
<!-- docker run --rm -it -v $(pwd):/usr/src jorgeandrada/doctoc --github -->


## Requirements

* Kubernetes >= 1.14
* [Helm][] >= v3.4.2

## Installing

### Install released version using Helm repository

**N.B.** You may or may not need/wish to install the chart into a specific namespace, 
in which case you may need to augment the commands below.

<!--
* Add the Elastic Helm charts repo:
`helm repo add elastic https://helm.elastic.co`

* Install it:
  - with Helm 3: `helm install filebeat elastic/filebeat`
  - with Helm 2 (deprecated): `helm install --name filebeat elastic/filebeat`
-->

### Install development version using master branch

* Clone the git repo: `git clone git@github.com:apache/tika-helm.git`

* Install it:
  - with Helm 3: `helm install tika . --set image.tag=latest-full`
  - with Helm 2 (deprecated): `helm install --name tika . --set image.tag=latest-full`


## Upgrading

Please always check [CHANGELOG.md][] and [BREAKING_CHANGES.md][] before
upgrading to a new chart version.


## Usage notes

* TODO


## Configuration

| Parameter                      | Description                                                                                                                                                                  | Default                            |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| `...`             | ...                                                                                        | ...               |

### Deprecated

| Parameter            | Description                                                                                                                                          | Default |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| `...`           | ...                                                                                                    | `...`    |

## FAQ

None yet...

## Contributing

Please check [CONTRIBUTING.md][] before any contribution or for any questions
about our development and testing process.

## More Information

For more infomation on Apache Tika Server, go to the [Apache Tika Server documentation][].

For more information on Apache Tika, go to the official [Apache Tika][] project website.

For more information on the Apache Software Foundation, go to the [Apache Software Foundation][] website.

## Authors

Apache Tika Dev Team (dev@tika.apache.org)

# License
The code is licensed permissively under the [Apache License v2.0][].

[Apache License v2.0]: https://www.apache.org/licenses/LICENSE-2.0.html
[Apache Software Foundation]: http://apache.org
[Apache Tika]: https://tika.apache.org
[Apache Tika Server documentation]: https://cwiki.apache.org/confluence/display/TIKA/TikaServer
[BREAKING_CHANGES.md]: https://github.com/apache/tika-helm/blob/master/BREAKING_CHANGES.md
[CHANGELOG.md]: https://github.com/apache/tika-helm/blob/master/CHANGELOG.md
[Helm chart]: https://helm.sh/docs/topics/charts/
[Kubernetes]: https://kubernetes.io/
[helm]: https://helm.sh
[latest release]: https://github.com/apache/tika-helm/releases