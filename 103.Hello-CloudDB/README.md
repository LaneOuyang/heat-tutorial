# Hello CloudDB Stack
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. The world's simplest Rackspace CloudDB Template

Use your favorite editor to create this file (named 'hello-clouddb.template'):

```shell
heat_template_version: 2013-05-23

resources:
  db: # You can name this whatever you prefer
    type: "OS::Trove::Instance"
    properties:
      name: My Test Database
      flavor: 1GB Instance
      size: 1
      databases:
      - name: testdb
      users:
      - name: testuser
        password: password
        databases:
        - testdb
```
</br>
### 3. Spin It Up!

```shell
heat stack-create CloudDB-Stack --template-file hello-clouddb.template
```

You should get a list of your stacks, including one with a `stack_name` of "CloudDB-Stack" with a `stack_status` of `CREATE_IN_PROGRESS`.
</br>
### 4. Check In On It

```shell
heat stack-list
```

If everything goes as planned it will have a status of `CREATE_IN_PROGRESS` for a bit, followed by `CREATE_COMPLETE`. Just re-run this command until you see `CREATE_COMPLETE`.

__Congratulations!__ You have successfully spun up a Cloud Database Stack. Now that we've proved we can spin one up, there's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat stack-delete CloudDB-Stack
```

You should see the status reported as `DELETE_IN_PROGRESS`. If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

__Cloud Database: Want to know more?__ For a complete list of properties see the <a href="http://docs.openstack.org/developer/heat/template_guide/openstack.html#OS::Trove::Instance" target="_blank">OpenStack documentation</a>

If you're not sure where to go next, try [the next tutorial](/104.Hello-CloudDNS).
