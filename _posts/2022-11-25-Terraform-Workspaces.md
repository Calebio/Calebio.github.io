---
layout: post
title: Terraform Workspaces
subtitle: How to create and manage Terraform workspaces
categories: Site
tags: [Terraform, Workspaces, Terraform Workspaces]
banner: "assets/images/banners/terraLogo.jpg"
---


Terraform Workspaces are isolated versions of the terraform state. With Terraform workspace, you can deploy multiple versions of the same environment having different configuration counts and variable definitions.<br/>

This can be useful when you need to deploy to either staging, testing or production environment without affecting other environments.<br/>

Typically, these workspaces will be tied to branch names and source control.<br/>

Terraform Workspaces come in handy for smaller deployments. It becomes difficult to manage if your deployments become too big and has a lot of engineers working on it so you may want to look into distinct environments, but for most deployments, workspaces work extremely well and are very simple to manage.<br/>


### Create a variable for reference
Before we get on to the creation of workspaces I will create a variable to specify which workspace I want to create resources in. if you don’t already have a `variable.tf` file, go ahead to create it within your working directory, and paste the code below: 
```
 variable "env"{
   type = string
   default = "dev"
   description = "Env I am deploying to "
 }
```


### Backends that support multiple terraform workspace
Here you can check on Terraform docs page to find out backends that support multiple Terraform workspaces. [Terraform Workspace Docs](https://developer.hashicorp.com/terraform/language/state/workspaces) <br/>
But for the most part, many backends support multiple workspaces on Terraform.
So be sure to consult the docs before you implement workspaces in your workflow to verify that your backend is supported


### How to create a terraform workspace
Below we will walk through the steps of creating multiple workspaces within Terraform and our examples will be Development and Production workspaces. I already have configurations within my terraform code specifying what goes to production and what goes to development.<br/>

Here I will create the first workspace and call it `dev`
`terraform workspace new dev`
Your output for this will be:
![output=> dev workspace](/assets/images/banners/created-dev-workspace.jpg "Output dev")

once a workspace is created you will automatically be moved to the workspace. <br/>

Notice the syntax: `terraform workspace new <workspace name>`

Next, we will create the production workspace and call it `prod`
`terraform workspace new prod`


So to check which workspace you are in `terraform workspace show`. <br/>

To get a list of all the workspaces you have available `terraform workspace list` or you can see it here on your project. You can also find it in your folders here:
![image](/assets/images/banners/list-workspaces-folder.jpg "list folder")

### Move between workspaces
Now to move between workspaces here's the command syntax: <br/>
`terraform workspace select <workspace name>`

So in my case, I’m moving to the dev workspace: `terraform workspace select dev` <br/>

Now I’m in my dev workspace I can apply my configurations for the dev workspace: <br/>
`terraform apply -auto-approve` <br/>
OR <br/>
`terraform apply -auto-approve -var="env=dev"` I used this to explicitly specify that I’m deploying to the dev workspace.<br/>

So in case you want to deploy to the `prod` workspace, you can move this “Move between workspaces” and replicate the steps for the `prod` workspace.
