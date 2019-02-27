---
layout: post
title:  What you need to run the examples
category: blog-post 
description: This post enumerates what you will need to run the blog&#39;s examples. It also points at relevant installation instructions and provides some hints how to use a free Azure account for experiments.
---

## You need an Azure account 

If you don&#39;t have one, here are instructions how to create one [Create your Azure free account](https://azure.microsoft.com/en-us/free/){:target="_blank"}. You will get  of credit before Azure will ask you to convert it to Pay-As-You-Go account, which should last you for a while if you are careful (see next point).

## You need an Azure cluster in the account

Before we point you at the instructions for how to create a cluster here is an important point. **_Kusto&#39;s amazing power comes at a price_**. The price is that clusters to perform must run on multiple expensive VMs. If you look at [Kusto cost estimator](https://dataexplorer.azure.com/AzureDataExplorerCostEstimator.html){:target="_blank"}{:target="_blank"} you will see that the smallest D11 cluster costs about $700/month, and that is even if you put no data in it. **_So for your free account to last a while follow these steps_** 

- Start creating a cluster in the portal as described in [Create an ADE cluster and database](https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-portal){:target="_blank"} QuickStart. 
    
- On the page  where you name the cluster make sure the you select the smallest and least expensive configuration called D11_v2. 
    
- Once the cluster has been successfully  created **stop it** in the portal: select the cluster resource, go to the Overview blade, and click the Stop button in the header.
    
- Turn the cluster on only when you are using it for test and experiments, and turn it off after you are done. When cluster in stopped you are not paying for its resources. 


## You need a database in the cluster

To create a DB you need to Start the cluster. Once it is running follow steps described in the same  [Create an ADE cluster and database](https://docs.microsoft.com/en-us/azure/data-explorer/create-cluster-database-portal){:target="_blank"} QuickStart. 

Once the DB has been created you may want to do the following
- In the portal select the cluster resource 
- In the resource blade select *Query*, which should open the Kusto Web Explorer
- In the query pane of the Explorer type ```.show database [yourDBname] principals```
- Select the query and click the Run button in the header. 

You should see something like the screen below.



![DB Principals]({{site.baseurl}}/assets/img/db-principals.jpg "DB Principals")

Notice that you are listed as the only principal (administrator in this case) that has access to that DB. In the post about Kusto REST APIs we will add an AAD application as another principal. 

## You need VS Code 

We use VS Code to do all code development and tests. We even use it to edit this blog. It runs equally well on Macs, Linux and Windows. If you don't have it you can find installation instruction [here](https://code.visualstudio.com/Download){:target="_blank"}. Install it, uses it, and get hooked on it :-). 

## You will need Azure CLI

We use the CLI for application deployment, and in other cases use it for resource management instead of the portal. You can find instructions how to install it [here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest){:target="_blank"}. 

## You may want to install PowerShell Core

We run the Azure CLI and Docker commands in a PowerShell console. PowerShell Core runs on Windows, Linux and Macs. You can find instructions how to install it [here](https://github.com/powershell/powershell){:target="_blank"}. 

We use PowerShell because it has a powerful scripting language and a convenient mechanisms to deal with JSON responses from the CLI commands. 
