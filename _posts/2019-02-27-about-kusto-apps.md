---
layout: post
title:  About Kusto apps
category: blog-post 
description: > # this means to ignore newlines until "baseurl:"
    There are differences between applications that interact with Kusto and  

    - run on a user's box in the browse or as executables, or
  
    - run in the cloud as intermediaries between Kusto and other applications.

    This post outlines those differences, and explains why we have been focusing on the second kind of applications - those that run in the cloud.
---

There are differences between applications interacting with Kusto clusters that **_(1)_** run on a user's box as executables or in the browser, and **_(2)_** services running in the cloud as intermediaries between Kusto and other applications.

Applications running on individual Kusto users' boxes, often referred to as local, almost always

- Authenticate with user's credentials and use user's token to interact with Kusto, and 
- Access cluster's physical data models (cluster BDs and tables as they are defined in the cluster's schema), and 
- Don't face serious concurrency and scalability issues as they are dedicated to one user commonly working on one task at a time. 
  
The intermediary services are often referred to as boundary services, proxies, or front-ends. On the inbound they expose REST client interface and on the outbound they communicate with one or more Kusto clusters. Common use cases include

> **_Ingestion pipelines_** are load-balanced collections of identical nodes that receive incoming Kusto table entries from some data sources (e.g. IOT devices), group them by type (DB + table), store/stage those groups in Azure storage (e.g. blobs), and then periodically submit the staged collections to Kusto for ingestion.

> **_Authenticating proxies_** authenticate and authorize users utilizing a non-Azure mechanism (e.g. Auth0 service) and then pass their request to Kusto cluster under their own AAD identity. More sophisticated proxies will also filter user requests based on some access rules. 

> **_Custom API proxies_** are most often parts of larger solutions that store different metrics, events and logs in Kusto, and provide access to that data using a custom API. These proxies translate custom queries to Kusto queries on the inbound, and transform Kusto responses to the custom responses on the outbound. 

> **_Multi-tenant front-ends_** are the most complex boundary services. They allow multiple tenants to share one of more Kusto clusters. At a high level there are two kinds of multi-tenant solutions **_(1)_** those with fixed data schema shared by all tenants, and **_(2)_** those where tenants can defined their own schemas. The Azure Application Insights is a service of both kinds. On one hand it stores predefined-type data (events, logs, exceptions, metrics) from thousands of applications in shared clusters, DBs and tables. On the hand it also allows users to ingest their own types to dedicated tables. 

The boundary services share common traits and challenges

- They must handle concurrency not only by processing multiple concurrent requests, but also by coordinating with other instances created to scale up 
- They should scale up and down depending on demand
- Their deployment and monitoring can be complex 
- They often need to authenticate an authorize requests and then access Kusto resources on behalf of the client
- They may analyze, filter and transform requests before submitting them to Kusto, and analyze and transform Kusto replies
- They often present a logical data model that is different from the underlying physical data model, which results in additional request and response transformations, and 
- In particular in multi-tenant solutions, they may need to manage Kusto resource allocation and sharing.

All of the above makes boundary services complex on one hand, but interesting on the other hand. 

Many of the initial posts and discussions in this blog applies to both local and boundary applications. However as we move forward we will focus on boundary services, as this is what we have been mostly developing.  
