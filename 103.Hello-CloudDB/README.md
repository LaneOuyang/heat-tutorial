# Hello CloudDB Stack
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
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
    type: "Rackspace::Cloud::DBInstance"
    properties:
      InstanceName: My Test Database
      FlavorRef: 1GB Instance
      VolumeSize: 1
      Databases:
      - Name: testdb
      Users:
      - Name: testuser
        Password: password
        Databases:
        - testdb
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create CloudDB-Stack --template-file hello-clouddb.template
```

You should get a list of your stacks, including one with a stack_name of "CloudDB-Stack" with a stack_status of "CREATE_IN_PROGRESS".
</br>
### 4. Check In On It

```shell
heat -k stack-list
```

If everything goes as planned it will have a status of "CREATE_IN_PROGRESS" for a bit, followed by "CREATE_COMPLETE". Just re-run this command until you see CREATE_COMPLETE.

__Congratulations!__ You have successfully spun up a Cloud Database Stack. Now that we've proved we can spin one up, there's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat -k stack-delete CloudDB-Stack
```

You should see the status reported as "DELETE_IN_PROGRESS". If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

__Cloud Database: Want to know more?__ For a complete list of properties, the source of truth is [the code](https://github.com/openstack/heat/blob/master/contrib/rackspace/heat/engine/plugins/clouddatabase.py). Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list:

```yaml
InstanceName: Your Cloud Database Name  # REQUIRED. 255 max characters.
FlavorRef: 1GB Instance                 # REQUIRED. Valid values: 1GB Instance, 2GB Instance, 4GB Instance, 8GB Instance, 16GB Instance
VolumeSize: 2                           # REQUIRED. Valid values: 1 - 150 (Gigabytes)
Databases:                              # Value must be a List.
- Name: testdb1                         # REQUIRED. Must start and end with alphanumeric or underscore and can also contain: @, ?, #, space
  Character_set: utf8                   # Valid values: 
  Collate: utf8_generic_ci              # Valid values: 
- Name: testdb2
Users:                                  # Value must be a List.
- Name: user1                           # REQUIRED.
  Password: password1                   # REQUIRED.
  Host: '%'                             # '%' is the default. Used to restrict access to specific IP addresses
  Databases:                            # Value must be a List.
  - testdb1
  - testdb2
- Name: user2
  Password: password2
  Databases:
  - testdb1
```

If you're not sure where to go next, try [the next tutorial](/104.Hello-CloudDNS).
