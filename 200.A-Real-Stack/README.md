# Resource Groups
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. >>Sanity Check<< Local Env

```shell
heat -k stack-list
```

This should return a list of all of your stacks. If this is your first time using Heat, it should be an empty list. If this isn't working you might need to revisit the [README.md at the root of this repo](/).
</br>
</br>
### 2. The first step into scaling infrastructure

We have learned how to spin up individual pieces of our infrastructure, but Heat really shines when you use it to spin up and wire up all your infrastructure. Let's start with a basic example: spin up a load balancer and wire it up to two web server instances.

Use your favorite editor to create this file (named 'lb-compute.template'):

```shell
heat_template_version: 2013-05-23

resources:
  web_nodes:
    type: OS::Heat::ResourceGroup
    properties:
      count: 2
      resource_def:
        type: Rackspace::Cloud::Server
        properties:
          flavor: 1GB Standard Instance
          image: CentOS 6.4
          name: LB-Compute Web Nodes

  lb:
    type: Rackspace::Cloud::LoadBalancer
    properties:
      name: LB-Compute Load Balancer
      nodes:
      - addresses: { get_attr: [web_nodes, PrivateIp]} # This is where the
                                                       # wiring magic happens
        port: 80
        condition: ENABLED
      healthMonitor:
        attemptsBeforeDeactivation: 3
        delay: 10
        timeout: 120
        type: HTTP
      protocol: HTTP
      port: 80
      virtualIps:
      - type: PUBLIC

outputs:
  web_node_resources:
    description: The actual compute instance details
    value: { get_attr: [web_nodes, refs]}

  lb_public_ip:
    description: The public IP address of the load balancer
    value: { get_attr: [lb, PublicIp]}
```
</br>
### 3. Spin It Up!

```shell
heat -k stack-create LB-Compute-Example --template-file lb-compute.template
```

You should get a list of your stacks, including one with a `stack_name` of "LB-Compute-Example" with a `stack_status` of `CREATE_IN_PROGRESS`. Make sure the `stack_status` changes to `CREATE_COMPLETE` (using `heat -k stack-list`) before proceeding to the next step.
</br>
### 4. Validate the Load Balancer's Configuration

```shell
heat -k stack-show LB-Compute-Example
```

You should see details for two cloud servers as well as the public IP address for your load balancer.

__Congratulations!__ You have successfully spun up your first Resource Group. Of course there is no web server running on the web-nodes, so the only thing we can do at this point is something like ping the IP address (or maybe try a telnet connection to port 80). There's only one thing left to do...
</br>
</br>
### 5. Delete It!

```shell
heat -k stack-delete LB-Compute-Example
```

You should see the status reported as `DELETE_IN_PROGRESS`. If you check again in a minute or so you should eventually see that the stack is no longer in the list, which means it has been deleted.
</br>
</br>
### 6. CONGRATULATIONS! You're Done!

If you're not sure where to go next, try [the next tutorial](/coming-soon).
