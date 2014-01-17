# Hello CloudLB Stack
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. The world's simplest Rackspace CloudLB Template

Use your favorite editor to create this file (named 'hello-cloudlb.template'):

```shell
heat_template_version: 2013-05-23

resources:
  lb: # You can name this whatever you prefer
    type: "Rackspace::Cloud::LoadBalancer"
    properties:
      name: My Test Load Balancer
      healthMonitor:
        attemptsBeforeDeactivation: 3
        delay: 10
        timeout: 120
        type: HTTP
      nodes:
      - condition: ENABLED
        port: 80
      protocol: HTTP
      port: 80
      virtualIps:
      - type: PUBLIC
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create CloudLB-Stack --template-file hello-cloudlb.template
```

You should get a list of your stacks, including one with a stack_name of "CloudLB-Stack" with a stack_status of "CREATE_IN_PROGRESS".
</br>
### 4. Check In On It

```shell
heat -k stack-list
```

If everything goes as planned it will have a status of "CREATE_IN_PROGRESS" for a bit, followed by "CREATE_COMPLETE". Just re-run this command until you see CREATE_COMPLETE.

__Congratulations!__ You have successfully spun up a Cloud Load Balancer Stack. Of course it's not a very useful stack: we haven't defined any compute instances to wire up to the load balancer. That will come in a future tutorial. For now there's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat -k stack-delete CloudLB-Stack
```

You should see the status reported as "DELETE_IN_PROGRESS". If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

__Cloud Load Balancer: Want to know more?__ For a complete list of properties, the source of truth is [the code](https://github.com/openstack/heat/blob/master/contrib/rackspace/heat/engine/plugins/cloud_loadbalancer.py). Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list:

```yaml
healthMonitor:                   # REQUIRED.
  attemptsBeforeDeactivation: 3  # REQUIRED. Valid values: 1 - 10
  delay: 10                      # REQUIRED. Valid values: 1 - 3,600
  timeout: 120                   # REQUIRED. Valid values: 1 - 300
  type: HTTP                     # REQUIRED. Valid values: CONNECT, HTTP, HTTPS
  bodyRegex: "."
  hostHeader: "header text"
  path: "/"
  statusRegex: "."
nodes:                           # REQUIRED. Value must be a List.
- condition: DISABLED            # REQUIRED. Valid values: ENABLED (default), DISABLED
  port: 80                       # REQUIRED.
  address: a-valid-address
  ref: a string
  type: PRIMARY                  # Valid values: PRIMARY, SECONDARY
  weight: 50                     # Valid values: 1 - 100
protocol: HTTPS                  # REQUIRED. Valid values: DNS_TCP, DNS_UDP, FTP, HTTP, HTTPS,
                                 #                         IMAPS, IMAPv4, LDAP, LDAPS, MYSQL,
                                 #                         POP3, POP3S, SMTP, TCP,
                                 #                         TCP_CLIENT_FIRST, UDP,
                                 #                         UDP_STREAM, SFTP
port: 80                         # REQUIRED. Valid values: Numeric
virtualIps:                      # REQUIRED. Value must be a List.
- type: PUBLIC                   # REQUIRED. Valid values: SERVICENET, PUBLIC
  ipVersion: IPV6                # REQUIRED. Valid values: IPV6 (default), IPV4
- type: PUBLIC
  ipVersion: IPV4
accessList:                      # Value must be a List.
- address: a-valid-address
  type: ALLOW                    # REQUIRED. Valid values: ALLOW, DENY
algorithm: ROUND_ROBIN           # See Rackspace documentation for valid values.
                                 # As of this writing: LEAST_CONNECTIONS, RANDOM, ROUND_ROBIN
                                 #                     WEIGHTED_LEAST_CONNECTIONS,
                                 #                     WEIGHTED_ROUND_ROBIN
connectionLogging: True          # Valid values: True, False
connectionThrottle:
  maxConnectionRate: 50          # Valid values: 0 - 100,000
  minConnections:  50            # Valid values: 1 - 1,000
  maxConnections: 50             # Valid values: 1 - 100,000
  rateInterval: 50               # Valid values: 1 - 3,600
contentCaching: ENABLED          # Valid values: ENABLED, DISABLED
errorPage: a string
halfClosed: True                 # Valid values: True, False
metadata:                        # Value must be a map
  your-key: your-value
name: Your Load Balancer Name
sessionPersistence: HTTP_COOKIE  # Valid values: HTTP_COOKIE, SOURCE_IP
sslTermination:
  securePort: 8443               # REQUIRED. Defaults to 443.
  privateKey: your-private-key   # REQUIRED.
  certificate: your-cert
  intermediateCertificate: your-other-cert
  secureTrafficOnly: True        # Valid values: False (default), True
timeout: 30                      # Valid values: 1 - 120
```

If you're not sure where to go next, try [the next tutorial](/103.Hello-CloudDB).
