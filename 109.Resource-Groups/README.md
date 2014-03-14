# Resource Groups
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. The first step into scaling infrastructure

So far, we've covered things that could be done without Heat. Now things get interesting! One of the cool advantages of Heat is the ability to spin up a whole army of clones to do your bidding. For that we use Resource Groups.

Use your favorite editor to create this file (named 'resource-groups.template'):

```shell
heat_template_version: 2013-05-23

resources:
  compute_instances:
    type: OS::Heat::ResourceGroup
    properties:
      count: 2
      resource_def:
        type: OS::Nova::Server
        properties:
          flavor: 1GB Standard Instance
          image: CentOS 6.5
          name: Scaling Out!

outputs:
  compute_resources:
    description: The actual compute instance details
    value: { get_attr: [compute_instances, refs]}
```
</br>
### 3. Spin It Up!

```shell
heat stack-create Resource-Group-Example --template-file resource-groups.template
```

You should get a list of your stacks, including one with a `stack_name` of "Resource-Group-Example" with a `stack_status` of `CREATE_IN_PROGRESS`. Make sure the `stack_status` changes to `CREATE_COMPLETE` (using `heat stack-list`) before proceeding to the next step.
</br>
### 4. Validate there is more than one instance

```shell
heat stack-show Resource-Group-Example
```

Look for the `refs` section under `outputs`. You should see two compute instances listed.
</br>
</br>
### 5. Extra Credit

Try using what you learned from the [update stack tutorial](/105.Update-Stack) to increase the number of compute instances from 2 to 3. _hint: yes, it's that easy!_

__Congratulations!__ You have successfully spun up your first Resource Group. Of course it's not very useful: what this template needs is a load balancer, which we'll add in [the next tutorial](/200.A-Real-Stack)! There's only one thing left to do...
</br>
</br>
### 6. Delete It!

```shell
heat stack-delete Resource-Group-Example
```

You should see the status reported as `DELETE_IN_PROGRESS`. If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 7. CONGRATULATIONS! You're Done!

__Resource Groups: Want to know more?__ Coming soon...

If you're not sure where to go next, try [the next tutorial](/200.A-Real-Stack).
