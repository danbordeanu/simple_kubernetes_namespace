# KUBERNETES NAMESPACE

A python-based helper capable of managing the Kubernetes namespace.

## Introduction

This lib will manage namespace in kubernetes cluster and setting quoata and limits.

## Instalation

### git clone

```bash
git clone https://github.com/danbordeanu/simple_kubernetes_namespace.git
```
### Prerequisites

1. python/pip
2. ssl certs from kube/config file

## Getting the ssl certs and populating the config.ini file

kube_namespace is using ssl cert file to authenticate, take a look at the __config.ini__ file

```bash
# certificate-authority-data
ssl_ca_cert_file: ssl/ca_cert.crt
# client-certificate-data
ssl_cert_file: ssl/cert_file.crt
# client-key-data
ssl_key_file: ssl/key_file.crt
```

The ssl certs are extracted from the kube config file:

Ex:

```
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: XXXX
    server: https://akscluster.xxxx.azmk8s.io:443
  name: akscluster
contexts:
- context:
    cluster: akscluster
    user: clusterAdmin_akscluster
  name: akscluster-admin
current-context: akscluster-admin
kind: Config
preferences: {}
users:
- name: clusterAdmin_akscluster
  user:
    client-certificate-data: XXXX
    client-key-data: XXXX
    token: XXXX

```

## Changing namespace limits and quota

All settings related with limits and quota are located in config.ini file

```
[namespace]
# namespace limits
name_limits :default-limits
default_request_cpu: 100m
default_request_memory: 256Mi
default_limit_cpu: 4
default_limit_memory: 512Mi
min_cpu: 100m
min_memory: 16Mi
#namespace quota
name_quota: user-quota
requests_cpu: 24
requests_memory: 16Gi
limits_cpu: 24
limits_memory: 16Gi
requests_storage: 64Gi
services_nodeports: 10
```

More info https://kubernetes.io/docs/concepts/policy/resource-quotas/

__!!!NB!!!__ all certs file are base64 encoded.

### Installing pip packages

```bash
pip install -r requirements.txt
```


## Namespace

### create a new namespace

```python
from kube_namespace import NameSpaceManagement
from kube_auth_string import KubeAuthString
auth_stuff = KubeAuthString()
auth_dict = auth_stuff.give_me_auth_values()
namespace_management = NameSpaceManagement(auth_dict)
dat = namespace_management.namespace_create('my-staging-namespace', 'True')
```

### set quoata

```python
namespace_management.namespace_quota('my-staging-namespace')
```

### set limits

```python
namespace_management.namespace_limits_ng_exp('my-staging-namespace')
```

### update namespace

```python
dat = namespace_management.namespace_create('my-staging-namespace', 'True')
```

The 'True' values enables patching the already created namespace

### delete namespace

```python
namespace_management.namespace_delete('my-staging-namespace')
```
