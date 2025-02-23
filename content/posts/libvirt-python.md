+++
title = 'Pulumi Libvirt Python'
date = 2023-12-12T17:49:16-06:00
draft = false
+++
Spin Up a **Kubernetes** cluster using Pulumi libvirt and Ansible
<!--more-->

### Table of content:

 - [Prerequirements](#1)

 - [Write pulumi code](#2)

 - [Deploy](#3)

 - [Automation API](#4)

 <!-- headings -->

 <a id="1"></a>

 ### Prerequirements
In this example I'm gonna use python as language for Pulumi and Ansible for further provisioning of our created VM's.



 <!-- <a id="2"></a> -->

 ### Write IAS for pulumi


{{< highlight python >}}
def pulumi_program():
        #Vars
        user = 'dev'
        user_pass = 'pass'
        root_pass = 'p@ssw0rd'
        ubuntu_img = "file:///home/max/projects/KickStartLab/iso/ubuntu-cloudx64.img"
        img_size = 21474836480 #20 GB
        sub_cird = ["172.17.172.0/24"]
        nodes_ips = (["172.17.172.99"],)
{{< / highlight >}}

We can also use Jinja2 for templates in our cloud-init configs:
{{< highlight jinja >}}
#cloud-config
autoinstall:
  user-data:
    chpasswd:
      expire: false
      list:
        - root:{{ root_pass }}
users:
  - name: {{ user }}
    ssh_authorized_keys:
      - ssh-rsa {{ public-key }}
{{< / highlight >}}
 <a id="3"></a>

### Deploy
Now you can run **Pulumi Up** and watch the magic happen.

Second item content goes here


<a id="4"></a>
 ### Automation API

We can simply get rid of Pulumi's CLI and wrap our code in function to pass it inside *automation* class from pulumi
{{< highlight python >}}
stack = auto.create_or_select_stack(stack_name="dev",
                            project_name="libvirt_python",
                            program=pulumi_program)
# Also we need to add connection line to our stack config
stack.set_config("libvirt:uri", auto.ConfigValue
(value="qemu:///system"))

up_res = stack.up(on_output=print)
{{< / highlight >}}
