# Template Outputs
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat stack-list
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
  and outputs the server public IP address. Adding a `parameters` section
  would make this template more useful.

resources:
  compute_instance:
    type: "OS::Nova::Server"
    properties:
      flavor: 1GB Standard Instance
      image: CentOS 6.5
      name: Simplest Stack In The World

outputs:
  compute_public_ip:
    description: The public IP address to the cloud server
    value: { get_attr: [compute_instance, first_address]}
```
</br>
### 3. Spin It Up!

```shell
heat stack-create Single-Compute-Stack --template-file template-outputs.template
```

</br>
### 4. View Output

You'll need to wait until the stack has been successfully created (`heat stack-list` will provide your stack's status). Once you see a status of `CREATE_COMPLETE`, show the stack:

```shell
heat stack-show Single-Compute-Stack
```

You should see an `outputs` section. This section is a list of maps, each map in the list containing `description`, `output_key`, and `output_value`. The outputs section should have one map and, in the `output_value`, you will see your server's public IP address!

__Congratulations!__ You have successfully added outputs to your template, making the resulting stack much more useful! There's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat stack-delete Single-Compute-Stack
```

You should see the status reported as `DELETE_IN_PROGRESS`. If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

__Template Outputs: Want to know more?__ Along with the `outputs` section, this tutorial introduces one of <a href="http://docs.openstack.org/developer/heat/template_guide/hot_spec.html#hot-spec-intrinsic-functions" target="_blank">HOT's Intrinsic Functions</a>.

If you're not sure where to go next, try [the next tutorial](/108.Template-Parameters).
