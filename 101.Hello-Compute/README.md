# Hello Compute Stack
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. The world's simplest Heat Orchestration Template (HOT)
##### _...that actually spins up a resource_

Use your favorite editor to create this file (named 'hello-compute.template'):

```shell
heat_template_version: 2013-05-23

resources:
  compute_instance:
    type: "Rackspace::Cloud::Server"
    properties:
      flavor: 1GB Standard Instance
      image: CentOS 6.4
      name: Simplest Stack In The World
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create Single-Compute-Stack --template-file hello-compute.template
```

You should get a list of your stacks, including one with a stack_name of "Single-Compute-Stack" with a stack_status of "CREATE_IN_PROGRESS".
</br>
### 4. Check In On It

```shell
heat -k stack-list
```

If everything goes as planned it will have a status of "CREATE_IN_PROGRESS" for a bit, followed by "CREATE_COMPLETE". Just re-run this command until you see CREATE_COMPLETE.

__Congratulations!__ You have successfully spun up your first Heat Stack. Of course it's not a very useful stack: you don't even know it's IP address and you can't ssh into it. There's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat -k stack-delete Single-Compute-Stack
```

You should see the status reported as "DELETE_IN_PROGRESS". If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

__Compute Instances: Want to know more?__ For a complete list of properties, the source of truth is [the code](https://github.com/openstack/heat/blob/master/contrib/rackspace/heat/engine/plugins/cloud_server.py). Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list (required properties in __bold__):

  * __flavor__
  * __image__
  * user_data
  * key_name
  * Volumes (value type is a List)
  * name
