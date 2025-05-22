This chart can be used to create a new namespace and two secrets for the clusterstacks approach. It reads clouds.yaml files in its raw form either with username and password or with an application credential. The chart is intended to be used once per Openstack-Project/Tenant. It is meant to prepare one corresponding namespace in the cluster-API management cluster (1:1 relation between openstackproject and cluster-namespace). The recommended way to invoke the chart is:

```
helm upgrade -i <tenant>-credentials -n <tenant> --create-namespace https://github.com/SovereignCloudStack/openstack-csp-helper/releases/latest/download/openstack-csp-helper.tgz -f clouds.yaml
```

If you want to make the OVN Octavia LB provider the default for OCCM, pass `--set octavia_ovn=true` to the helm command.

If OpenStack API is protected by the certificate issued by custom CA, add `--set cacert="$(cat /path/to/cacert)"` to the helm command.

It is recommended to use application credentials in `clouds.yaml` (`auth_type: v3applicationcredential`), it is the preferred and more secure option.

If you opt to use clouds.yaml with password authentication (`auth_type: v3password`, default), that is also acceptable, but:
- Ensure that `project_id` is set, `project_name` (with `project_domain_name`) works only for CAPO, not for OCCM!
- Using `project_id` guarantees that both CAPO and OCCM function correctly with `v3password` authentication.
