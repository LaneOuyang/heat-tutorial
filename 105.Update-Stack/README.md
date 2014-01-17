# Update Stack Tutorial
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. Remember the [World's simplest Stack](/101.Hello-Compute)? We're going to start with that one again.

Use your favorite editor to create this file (named 'update-stack.template'):

```shell
heat_template_version: 2013-05-23

resources:
  compute_instance: # You can name this whatever you prefer
    type: "Rackspace::Cloud::Server"
    properties:
      flavor: 1GB Standard Instance
      image: CentOS 6.4
      name: Single Compute Instance
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create Update-StackExample --template-file update-stack.template
```

You should get a list of your stacks, including one with a `stack_name` of "Update-Stack-Example" with a `stack_status` of `CREATE_IN_PROGRESS`.
</br>
### 4. Check In On It

```shell
heat -k stack-list
```

If everything goes as planned it will have a status of `CREATE_IN_PROGRESS` for a bit, followed by `CREATE_COMPLETE`. Just re-run this command until you see `CREATE_COMPLETE`.

Now we have a Stack up and running, but what if we want to change something about its configuration? That's where stack-update comes in.
</br>
</br>
### 5. Update It!

Oops! It turns out we really need a 2 GB Performance instance. Fortunately Heat lets me update a stack simply by editing my original template and using the `stack-update` command. Edit the `flavor` property in the update-stack.template file:

```shell
heat_template_version: 2013-05-23

resources:
  compute_instance: # You can name this whatever you prefer
    type: "Rackspace::Cloud::Server"
    properties:
      flavor: 2 GB Performance
      image: CentOS 6.4
      name: High Performance Compute Instance
```

Save the file, then run:

```shell
heat -k stack-update Update-Stack-Example --template-file update-stack.template
```

You should see the `stack_status` in the resulting output is now `UPDATE_IN_PROGRESS`. You can check in on update progress using the shell command from step 4. Assuming the update is successful you will see the `stack_status` change to `UPDATE_COMPLETE`. Now that we've updated it, there is only one thing left to do...
</br>
</br>
### 6. Delete It!

```shell
heat -k stack-delete Update-Stack-Example
```

You should see the status reported as `DELETE_IN_PROGRESS`. If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

If you're not sure where to go next, try the next tutorial (Coming soon).
