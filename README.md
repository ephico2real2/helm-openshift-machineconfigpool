# helm-openshift-machineconfigpool
Helm to Chart to manage machineconfigpool on openshift

```

This is a Helm template for the machineconfigpool.yaml file that creates a MachineConfigPool resource for each entry in the machineConfigPools array in the values.yaml file.

Explanation:

{{- range .Values.machineConfigPools }} starts a loop over the machineConfigPools array in the values.yaml file.

name: {{ .mcpName }} sets the name of the MachineConfigPool to the value of the mcpName field in the current iteration of the loop.

values: {{ .machineRoles | toJson }} sets the value of the values field in the matchExpressions list to the value of the machineRoles 
field in the current iteration of the loop. The toJson function is used to convert the array to a JSON list.

nodeSelector: specifies the node selector for the MachineConfigPool. If the nodeSelector field is not present or is empty, the template will throw an error with the message "nodeSelector is required for all MachineConfigPools".

matchLabels: specifies the labels to match in the node selector. The nindent function is used to indent the block by six spaces.

paused: {{ .paused | default false }} sets the value of the paused field to the value of the paused field in the current iteration of the loop, or false if the paused field is not present. The default function is used to provide a default value.

--- is used to separate each MachineConfigPool resource created by the loop.

```

```

The Folders -  env/rnd/; env/uat/; env/qa/ etc represent the required folders containing values.yaml for rnd, uat and qa cluster in a mono-repo for GitOps.

The assumpption is that we are preserving the worker label, so we are using the additional labels such as 'custom' (node-role.kubernetes.io/custom) in conjuction with worker label to create the required mcp .e.g for any generic called "custom" will select the label "node-role.kubernetes.io/custom" and "node-role.kubernetes.io/worker".

please adjust values.yaml to suit your need.


```

rnd values.yaml 

```yaml

machineConfigPools:
  - mcpName: infra
    machineRoles:
      - worker
      - infra
    nodeSelector:
      key: node-role.kubernetes.io/infra
      value: ""
    paused: true
  - mcpName: app
    machineRoles:
      - worker
      - app
    maxUnavailable: 1
    nodeSelector:
      key: node-role.kubernetes.io/app
      value: "true"

```


`` helm template my-release --values sample-values.yaml . --debug  ``

```

In this example, we're using the machineConfigPools field to specify the configuration options for each MCP.

The code block is a YAML file defining a list of machineConfigPools that can be managed by the OpenShift cluster. Each machineConfigPool in the list has a name mcpName and a list of machineRoles that it can manage, including worker and custom roles. The nodeSelector field specifies a Kubernetes label and value that should be present on the nodes to be managed by this pool, with the exception of the infra pool which has a blank value. The paused field specifies whether the machineConfigPool should be paused or not.

In addition, the storage machineConfigPool specifies a maxUnavailable value of 1, which means only 1 node at a time in this pool can be unavailable. Finally, custom2 machineConfigPool has a different node selector key node-role.kubernetes.io/custom2 and doesn't specify a value, which means any value will be accepted for this label.

You can add more MCPs to the machineConfigPools field as needed, and the machineconfigpool.yaml template will automatically generate the corresponding YAML files for each MCP

```
