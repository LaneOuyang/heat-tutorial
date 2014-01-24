# Template Outputs
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. Getting meaningful output from the world's simplest Heat Orchestration Template (HOT)

Use your favorite editor to create this file (named 'template-outputs.template'):

```shell
heat_template_version: 2013-05-23

description: |
  Heat Orchestration Template that spins up a single Rackspace Cloud Server 
  and outputs the server's public IP address. Adding a `parameters` section
  would make this template more useful.

resources:
  compute_instance:
    type: "Rackspace::Cloud::Server"
    properties:
      flavor: 1GB Standard Instance
      image: CentOS 6.4
      name: Simplest Stack In The World

outputs:
  compute_public_ip:
    description: The public IP address to the cloud server
    value: { get_attr: [compute_instance, public_ip]}
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create Single-Compute-Stack --template-file template-outputs.template
```

</br>
### 4. View Output

You'll need to wait until the stack has been successfully created (`heat -k stack-list` will provide your stack's status). Once you see a status of `CREATE_COMPLETE`, show the stack:

```shell
heat -k stack-show Single-Compute-Stack
```

You should see an `outputs` section. This section is a list of maps, each map in the list containing `description`, `output_key`, and `output_value`. The outputs section should have one map and, in the `output_value`, you will see your server's public IP address!

__Congratulations!__ You have successfully added outputs to your template, making the resulting stack much more useful! There's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat -k stack-delete Single-Compute-Stack
```

You should see the status reported as `DELETE_IN_PROGRESS`. If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

__Template Outputs: Want to know more?__ Along with the `outputs` section, this tutorial introduces one of [HOT's Intrinsic Functions](http://docs.openstack.org/developer/heat/template_guide/hot_spec.html#hot-spec-intrinsic-functions). For a complete list of Intrinsic Functions, the source of truth is [the code](https://github.com/openstack/heat/blob/master/heat/engine/hot.py). Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list:

```yaml
{ get_param: <param_name>}                         # retrieves an entry by name from
                                                   # the template's `parameters` section

{ get_resource: <resource_name>}                   # retrieves an entry by name from
                                                   # the template's `resources` section

{ get_attr: [<resource_name>, <atttribute_name>]}  # retrieves an attribute's value from
                                                   # the named resource

str_replace:                                       # inserts a string defined by `template`
  template: <string template>                      # e.g. "http://%ip%/wordpress"
  params:                                          # the definition of the params in the template
    <param dictionary>                             # e.g. "%ip%": { get_attr: [ lb, public_ip ] }

```

If you're not sure where to go next, try [the next tutorial](/108.Template-Parameters).
