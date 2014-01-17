# Hello CloudDNS Stack
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. The world's simplest Rackspace CloudDNS Template

Use your favorite editor to create this file (named 'hello-clouddns.template'):

```shell
heat_template_version: 2013-05-23

resources:
  dns: # You can name this whatever you prefer
    type: "Rackspace::Cloud::DNS"
    properties:
      Coming: Soon
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create CloudDNS-Stack --template-file hello-clouddns.template
```

You should get a list of your stacks, including one with a stack_name of "CloudDNS-Stack" with a stack_status of "CREATE_IN_PROGRESS".
</br>
### 4. Check In On It

```shell
heat -k stack-list
```

If everything goes as planned it will have a status of "CREATE_IN_PROGRESS" for a bit, followed by "CREATE_COMPLETE". Just re-run this command until you see CREATE_COMPLETE.

__Congratulations!__ You have successfully spun up a Cloud DNS Stack. Now that we've proved we can spin one up, there's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat -k stack-delete CloudDNS-Stack
```

You should see the status reported as "DELETE_IN_PROGRESS". If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

__Cloud DNS: Want to know more?__ For a complete list of properties, the source of truth is [the code](https://github.com/openstack/heat/blob/master/contrib/rackspace/heat/engine/plugins/cloud_dns.py). Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list:

```yaml
Coming: Soon
```

If you're not sure where to go next, try [the next tutorial](/105.Update-Stack).
