# audittail-helm-library

A library chart is a type of Helm chart that defines chart primitives or definitions
which can be shared by Helm templates in other charts.
This allows users to share snippets of code that can be re-used across charts, avoiding repetition and keeping charts DRY.

It may be used on your Deployment as follows:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  ...
spec:
  ...
  template:
    ...
    spec:
      initContainers:
        - {{ include audittail.initContainer" | ident 10}}
      containers:
        - image: MY_APP_IMAGE
          args:
            - --audit-log-path={{ template "audittail.auditLogpath" }}
          name: my-app
          volumeMounts:
            - {{ include "audittail.volumeMount" | ident 12 }}
        # Mandatory: tails audit logs and outputs them to stdout
        # for the Splunk forwarder to pick up
        - {{ include "audittail.sidecarcontainer" | ident 10 }}
      volumes:
        - {{include "audittail.volume" | ident 10 }}

```

Just include the audittail helm library chart in their dependencies as they can any other helm dependency.
Example:

```yaml
---
dependencies:
  - name: audittail
    repository: https://<path to chart index>/audittail
    tags:
      - audit
    version: 0.1.0
