# Template Paramaters
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. Paramaterizing the world's simplest Heat Orchestration Template (HOT)

Use your favorite editor to create this file (named 'template-parameters.template'):

```shell
heat_template_version: 2013-05-23

description: |
  Heat Orchestration Template that spins up a single Rackspace Cloud Server,
  parameterized to allow setting flavor and image from a list of valid options.
  Adding a `outputs` section would make this template more useful.

parameters:
  compute_instance_flavor:
    description: flavor id for the compute instance
    type: String
    default: 1 GB Performance
    constraints:
      description: Must be a valid Rackspace Cloud Server flavor.
    - allowed_values:
      - 1 GB Performance
      - 2 GB Performance
      - 4 GB Performance
      - 8 GB Performance
      - 16 GB Performance

  compute_instance_image:
    description: Image name for the compute instance
    type: String
    default: CentOS 6.4
    constraints:
      description: Must be a valid Rackspace Cloud Server image name.
    - allowed_values:
      - CentOS 6.4
      - CentOS 5.10
      - Arch 2013.9
      - Ubuntu 13.10
      - Ubuntu 12.10

resources:
  compute_instance: # You can name this whatever you prefer
    type: "Rackspace::Cloud::Server"
    properties:
      flavor: { get_param: compute_instance_flavor }
      image: { get_param: compute_instance_image }
      name: Single Cloud Server compute instance

outputs:
  public_ip:
    description: The public IP address to the cloud server
    value: { get_attr: [compute_instance, PublicIp]}
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create Single-Compute-Stack --template-file template-parameters.template
```

</br>
### 4. View Output

You'll need to wait until the stack has been successfully created (`heat -k stack-list` will provide your stack's status). Once you see a status of `CREATE_COMPLETE`, show the stack:

```shell
heat -k stack-show Single-Compute-Stack
```

___Add details for showing Image and Flavor in outputs___

__Congratulations!__ You have successfully added and used parameters in your template, making the resulting stack much more useful! There's only one thing left to do...
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

__Template Parameters: Want to know more?__ Along with the `outputs` section, this tutorial introduces one of [HOT's Intrinsic Functions](http://docs.openstack.org/developer/heat/template_guide/hot_spec.html#hot-spec-intrinsic-functions). For a complete list of Intrinsic Functions, the source of truth is [the code](https://github.com/openstack/heat/blob/master/heat/engine/hot.py). Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list:

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
    <param dictionary>                             # e.g. "%ip%": { get_attr: [ lb, PublicIp ] }

```

If you're not sure where to go next, try [the next tutorial](coming soon...).
