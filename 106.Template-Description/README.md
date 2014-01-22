# Template Description
### Author: Pablo <paul.nelson@rackspace.com>
</br>
### 1. The world's simplest Heat Orchestration Template: Revisited

There is a section of the Heat Orchestration Template (HOT) that, while it does not change any resulting functionality, should always be present. In other words, the template description is important, but the Heat orchestration engine will process a template just fine without a `description` section.

Use your favorite editor to create this file (named 'template-description.template'):

```shell
heat_template_version: 2013-05-23

description: |
  Heat Orchestration Template that spins up a single Rackspace Cloud Server,
  intended to demonstrate the simplest HOT. Adding a `parameters` section and
  an `outputs` section would make this template more useful.

resources:
  compute_instance: # You can name this whatever you prefer
    type: "Rackspace::Cloud::Server"
    properties:
      flavor: 1GB Standard Instance
      image: CentOS 6.4
      name: Simplest Stack In The World
```

Since this does not change any functionality I have omitted the `heat` commands. However, this would be a good opportunity to see if you can spin up a stack, check its status, and delete it without any hints.

If you're not sure where to go next, try [the next tutorial](/107.Template-Outputs).
