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
  compute_flavor:
    description: flavor id for the compute instance
    type: String
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
    type: String
    default: CentOS 6.4
    constraints:
    - allowed_values:
      - CentOS 6.4
      - CentOS 5.10
      - Arch 2013.9
      - Ubuntu 13.10
      - Ubuntu 12.10
      description: Must be a valid Rackspace Cloud Server image name.

resources:
  compute_instance:
    type: "Rackspace::Cloud::Server"
    properties:
      flavor: { get_param: compute_flavor }
      image: { get_param: compute_image }
      name: Single Cloud Server compute instance

outputs:
  compute_public_ip:
    description: The public IP address to the cloud server
    value: { get_attr: [compute_instance, public_ip]}

  image_used:
    description: The image this compute instance was built upon
    value: { get_attr: [compute_instance, image]}

  flavor_used:
    description: The flavor used to build this compute instance
    value: { get_attr: [compute_instance, flavor]}
```
</br>
### 3. Spin It Up!

We could just go with the defaults for the parameters but what would that prove?! Let's be specific:

```shell
heat -k stack-create Single-Compute-Stack --template-file template-parameters.template --parameters compute_flavor="2 GB Performance";compute_image="Arch 2013.9"
```

</br>
### 4. View Output

You'll need to wait until the stack has been successfully created (`heat -k stack-list` will provide your stack's status). Once you see a status of `CREATE_COMPLETE`, show the stack:

```shell
heat -k stack-show Single-Compute-Stack
```

In the `outputs` section you should see `image` listed as "Arch 2013.9" and `flavor` listed as "2 GB Performance".

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

__Template Parameters: Want to know more?__ Along with the `parameters` section, this tutorial introduces some of Cloud Server's attributes. For a complete list of attributes, the source of truth is <a href="https://github.com/openstack/heat/blob/master/contrib/rackspace/heat/engine/plugins/cloud_server.py" target="_blank">the code</a>. Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list:

* distro
* flavor
* image
* private_ip
* private_key
* public_ip
* server

If you're not sure where to go next, try [the next tutorial](/109.Resource-Groups).
