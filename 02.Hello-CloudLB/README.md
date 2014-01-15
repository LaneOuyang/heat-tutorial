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
  lb:
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

__Congratulations!__ You have successfully spun up a Cloud Load Balancer Stack. Of course it's not a very useful stack: you don't even know it's IP address and you can't ssh into it. There's only one thing left to do...
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

__Cloud Load Balancer: Want to know more?__ For a complete list of properties, the source of truth is [the code](https://github.com/openstack/heat/blob/master/contrib/rackspace/heat/engine/plugins/cloud_loadbalancer.py). Here is the guaranteed-to-be-out-of-date-as-soon-as-it-is-published list (required properties in __bold__, ranges in parenthesis):

  * __healthMonitor__
    * __attemptsBeforeDeactivation__ (1 - 10)
    * __delay__ (1 - 3,600)
    * __timeout__ (1 - 300)
    * __type__ (CONNECT, HTTP, HTTPS)
    * bodyRegex
    * hostHeader
    * path
    * statusRegex
  * __nodes__
    * __condition__ (ENABLED, DISABLED, defaults to ENABLED)
    * __port__
    * address
    * ref
    * type (PRIMARY, SECONDARY)
    * weight (1 - 100)
  * __protocol__ (DNS_TCP, DNS_UDP, FTP, HTTP, HTTPS, IMAPS, IMAPv4, LDAP, LDAPS, MYSQL, POP3, POP3S, SMTP, TCP, TCP_CLIENT_FIRST, UDP, UDP_STREAM, SFTP)
  * __port__
  * __virtualIps__
    * __type__ (SERVICENET, PUBLIC)
    * ipVersion (IPV6, IPV4, defaults to IPV6)
  * accessList
    * __address__
    * __type__ (ALLOW, DENY)
  * algorithm
  * connectionLogging
  * connectionThrottle
    * maxConnectionRate (0 - 100,000)
    * minConnections (1 - 1,000)
    * maxConnections (1 - 100,000)
    * rateInterval (1 - 3,600)
  * contentCaching (ENABLED, DISABLED)
  * errorPage
  * halfClosed
  * metadata
  * name
  * sessionPersistence (HTTP_COOKIE, SOURCE_IP)
  * sslTermination
    * __securePort__ (defaults to 443)
    * __privateKey__
    * __certificate__
    * intermediateCertificate
    * secureTrafficOnly (defaults to False)
  * timeout (1 - 120)
