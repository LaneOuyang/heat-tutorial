# Template Paramaters
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat stack-list
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
  compute_flavor:
    description: flavor id for the compute instance
    type: string
    default: 1 GB Performance
    constraints:
    - allowed_values:
      - 1 GB Performance
      - 2 GB Performance
      - 4 GB Performance
      - 8 GB Performance
      - 16 GB Performance
      description: Must be a valid Rackspace Cloud Server flavor.

  compute_image:
    description: Image name for the compute instance
    type: string
    default: CentOS 6.5
    constraints:
    - allowed_values:
      - CentOS 6.5
      - CentOS 5.10
      - Arch 2014.2 (PVHVM)
      - Ubuntu 13.10 (Saucy Salamander)
      description: Must be a valid Rackspace Cloud Server image name.

resources:
  compute_instance:
    type: OS::Nova::Server
    properties:
      config_drive: "true"
      user_data_format: RAW
      flavor: { get_param: compute_flavor }
      image: { get_param: compute_image }
      name: Single Cloud Server compute instance

outputs:
  server_details:
    description: All details for the server
    value: { get_attr: [compute_instance, show]}
```
</br>
### 3. Spin It Up!

We could just go with the defaults for the parameters but what would that prove?! Let's be specific:

```shell
heat stack-create Single-Compute-Stack --template-file template-parameters.template --parameters compute_flavor="2 GB Performance";compute_image="Arch 2013.9"
```

</br>
### 4. View Output

You'll need to wait until the stack has been successfully created (`heat stack-list` will provide your stack's status). Once you see a status of `CREATE_COMPLETE`, show the stack:

```shell
heat stack-show Single-Compute-Stack
```

In the `outputs` section you should see an `image` section and a `flavor` section among many other server details.

__Congratulations!__ You have successfully added and used parameters in your template, making the resulting stack much more useful! There's only one thing left to do...
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

__Template Parameters: Want to know more?__ Along with the `parameters` section, this tutorial introduces some of Cloud Server's attributes. For more information on parameters see the <a href="http://docs.openstack.org/developer/heat/template_guide/hot_spec.html#parameters-section" target="_blank">HoT specification</a>. For a full list of Cloud Server attributes see the <a href="http://docs.openstack.org/developer/heat/template_guide/openstack.html#OS::Nova::Server" target="_blank">OpenStack documentation</a>.

If you're not sure where to go next, try [the next tutorial](/109.Resource-Groups).
