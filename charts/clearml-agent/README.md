# ClearML Kubernetes Agent

![Version: 2.0.0](https://img.shields.io/badge/Version-2.0.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.24](https://img.shields.io/badge/AppVersion-1.24-informational?style=flat-square)

MLOps platform

**Homepage:** <https://clear.ml>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| valeriano-manassero |  | <https://github.com/valeriano-manassero> |

## Introduction

The **clearml-agent** is the Kubernetes agent for for [ClearML](https://github.com/allegroai/clearml).
It allows you to schedule distributed experiments on a Kubernetes cluster.

## Source Code

* <https://github.com/allegroai/clearml-helm-charts>
* <https://github.com/allegroai/clearml>

## Requirements

Kubernetes: `>= 1.19.0-0 < 1.25.0-0`

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| agentk8sglue | object | `{"apiServerUrlReference":"https://api.clear.ml","clearmlcheckCertificate":true,"defaultContainerImage":"ubuntu:18.04","extraEnvs":[],"fileServerUrlReference":"https://files.clear.ml","id":"k8s-agent","image":{"repository":"allegroai/clearml-agent-k8s-base","tag":"1.24-18"},"maxPods":10,"podTemplate":{"env":[],"nodeSelector":{},"resources":{},"tolerations":[],"volumeMounts":[],"volumes":[]},"queue":"default","replicaCount":1,"serviceAccountName":"default","webServerUrlReference":"https://app.clear.ml"}` | This agent will spawn queued experiments in new pods, a good use case is to combine this with GPU autoscaling nodes. https://github.com/allegroai/clearml-agent/tree/master/docker/k8s-glue |
| agentk8sglue.apiServerUrlReference | string | `"https://api.clear.ml"` | Reference to Api server url |
| agentk8sglue.clearmlcheckCertificate | bool | `true` | Check certificates validity for evefry UrlReference below. |
| agentk8sglue.defaultContainerImage | string | `"ubuntu:18.04"` | default container image for ClearML Task pod |
| agentk8sglue.extraEnvs | list | `[]` | Environment variables to be exposed in the agentk8sglue pods |
| agentk8sglue.fileServerUrlReference | string | `"https://files.clear.ml"` | Reference to File server url |
| agentk8sglue.id | string | `"k8s-agent"` | ClearML worker ID (must be unique across the entire ClearMLenvironment) |
| agentk8sglue.image | object | `{"repository":"allegroai/clearml-agent-k8s-base","tag":"1.24-18"}` | Glue Agent image configuration |
| agentk8sglue.maxPods | int | `10` | maximum concurrent consume ClearML Task pod |
| agentk8sglue.podTemplate | object | `{"env":[],"nodeSelector":{},"resources":{},"tolerations":[],"volumeMounts":[],"volumes":[]}` | template for pods spawned to consume ClearML Task |
| agentk8sglue.podTemplate.env | list | `[]` | environment variables for pods spawned to consume ClearML Task (example in values.yaml comments) |
| agentk8sglue.podTemplate.nodeSelector | object | `{}` | nodeSelector setup for pods spawned to consume ClearML Task (example in values.yaml comments) |
| agentk8sglue.podTemplate.resources | object | `{}` | resources declaration for pods spawned to consume ClearML Task (example in values.yaml comments) |
| agentk8sglue.podTemplate.tolerations | list | `[]` | tolerations setup for pods spawned to consume ClearML Task (example in values.yaml comments) |
| agentk8sglue.podTemplate.volumeMounts | list | `[]` | volumeMounts definition for pods spawned to consume ClearML Task (example in values.yaml comments) |
| agentk8sglue.podTemplate.volumes | list | `[]` | volumes definition for pods spawned to consume ClearML Task (example in values.yaml comments) |
| agentk8sglue.queue | string | `"default"` | ClearML queue this agent will consume |
| agentk8sglue.replicaCount | int | `1` | Glue Agent number of pods |
| agentk8sglue.serviceAccountName | string | `"default"` | serviceAccountName for pods spawned to consume ClearML Task |
| agentk8sglue.webServerUrlReference | string | `"https://app.clear.ml"` | Reference to Web server url |
| clearml | object | `{"agentk8sglueKey":"ACCESSKEY","agentk8sglueSecret":"SECRETKEY","clearmlConfig":"sdk {\n}","existingAgentk8sglueSecret":"","existingClearmlConfigSecret":""}` | ClearMl generic configurations |
| clearml.agentk8sglueKey | string | `"ACCESSKEY"` | Agent k8s Glue basic auth key |
| clearml.agentk8sglueSecret | string | `"SECRETKEY"` | Agent k8s Glue basic auth secret |
| clearml.clearmlConfig | string | `"sdk {\n}"` | ClearML configuration file |
| clearml.existingAgentk8sglueSecret | string | `""` | If this is set, chart will not generate a secret but will use what is defined here |
| clearml.existingClearmlConfigSecret | string | `""` | If this is set, chart will not generate a secret but will use what is defined here |
| imageCredentials | object | `{"email":"someone@host.com","enabled":false,"existingSecret":"","password":"pwd","registry":"docker.io","username":"someone"}` | Private image registry configuration |
| imageCredentials.email | string | `"someone@host.com"` | Email |
| imageCredentials.enabled | bool | `false` | Use private authentication mode |
| imageCredentials.existingSecret | string | `""` | If this is set, chart will not generate a secret but will use what is defined here |
| imageCredentials.password | string | `"pwd"` | Registry password |
| imageCredentials.registry | string | `"docker.io"` | Registry name |
| imageCredentials.username | string | `"someone"` | Registry username |

# Upgrading Chart

### From v1.x to v2.x

Chart 1.x was under the assumption that all mounted volumes would be PVC's. Version > 2.x allows for more flexibility and will inject the yaml from podTemplate.volumes and podtemplate.volumeMounts directly.

v1.x
```
    volumes:
     - name: "yourvolume"
       path: "/yourpath"
```

v2.x
```
    volumes:
     - name: "yourvolume"
       persistentVolumeClaim:
         claimName: "yourvolume"
    volumeMounts:
     - name: "yourvolume"
       mountPath: "/yourpath"
```
