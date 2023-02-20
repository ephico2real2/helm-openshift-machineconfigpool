# helm-openshift-machineconfigpool
Helm to Chart to manage machineconfigpool on openshift

```

In this version, we're defining anchor aliases for commonly-used MCP fields using the define function. We're defining mcpDefaults to set default values for maxUnavailable, paused, and nodeSelector, and we're using the & operator to create an anchor alias called mcpDefaults.

We're also defining mcpName and machineRoles using the define function, and using them as template functions to output the MCP name and machine roles, respectively.

In the range loop, we're using the dict function to create a new $mcpConfig dictionary for the current MCP, which includes the MCP name, machine roles, and the default values from mcpDefaults.

We're then using the merge function to merge the $mcpConfig dictionary with the configuration options provided in the values.yaml file, using the | default (dict) syntax to ensure that the merge operation is performed only if $element is not nil.

We're using the template function to output the MCP name and machine roles, and we're using the toYaml function and the nindent function to format the machine roles list and node selector dictionary, respectively.

We're also using a lookup function to verify that the node selector label specified in the values.yaml file exists on the nodes in the cluster. We're using the lookup function to retrieve the value of the node label, and we're using the if function and the fail function to fail the template if the label is not found.

```

sample values.yaml

```
In this example, we're using the machineConfigPools field to specify the configuration options for each MCP. We have two MCPs: one called infra and one called workers.

For the infra MCP, we're setting the machine roles to worker and infra, the node selector to node-role.kubernetes.io/infra, and the paused field to true.

For the workers MCP, we're setting the machine roles to worker, the maxUnavailable field to 1, and the node selector to node-role.kubernetes.io/worker.

You can add more MCPs to the machineConfigPools field as needed, and the machineconfigpool.yaml template will automatically generate the corresponding YAML files for each MCP

```
