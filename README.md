# multi-arch-konflux-example
A simple example for using multi-arch pipelines on the Red Hat Konflux clusters

## Adding New Components to Konflux
Each test component in the Konflux clusters comes from this repository and has its corresponding pipelines stored in this repo's `.tekton` directory.
In order to prevent all component pipelines from being run on every cluster (including the clusters they don't belong on), the following steps need to occur:
1. After creating a new component in Konflux, find the component's corresponding `Repository` resource on its cluster using the OpenShift CLI or UI.
2. Add a parameter called 'cluster' to the `Repository` resource's YAML file and set its value to be the name of the cluster the component is located on (e.g. internal-staging).
3. Apply the above changes to the YAML file.
4. For both the pull-request and push pipeline files created for the component, add the following expression to the `pipelinesascode.tekton.dev/on-cel-expressions` annotation:
  `&& "{{ cluster }}" == "<name of cluster>"` 
