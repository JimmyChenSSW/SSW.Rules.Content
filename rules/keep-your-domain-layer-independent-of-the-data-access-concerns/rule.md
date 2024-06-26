---
seoDescription: Keep your domain layer independent of data access concerns by separating configuration and annotations from your domain model.
type: rule
archivedreason:
title: Do you keep your domain layer independent of the data access concerns?
guid: 60e17663-19ec-415f-8296-96ee730835a2
uri: keep-your-domain-layer-independent-of-the-data-access-concerns
created: 2019-04-09T23:15:40.0000000Z
authors:
  - title: Jason Taylor
    url: https://ssw.com.au/people/jason-taylor
  - title: Matt Wicks
    url: https://ssw.com.au/people/matt-wicks
related: []
redirects:
  - do-you-keep-your-domain-layer-independent-of-the-data-access-concerns
---

The domain layer should be independent of data access concerns. The domain layer should only change when something within the domain changes, not when the data access technology changes. Doing so ensures that the system will be easier to maintain well into the future since changes to data access technologies won't impact the domain, and vice versa.

This is often a problem when building systems that leverage Entity Framework, as it's common for data annotations to be added to the domain model. Data annotations, such as the Required or MinLength attributes, support validation and help Entity Framework to map objects into the relational model. In the next example, data annotations are used within the domain model:

<!--endintro-->

::: bad  
![Bad Example: Domain is cluttered with data annotations](domain-layer-1.png)  
:::

As you can see in the above example, the domain is cluttered with data annotations. If the data access technology changes, we will likely need to change all entities as all entities will have data annotations. In the following example, we will remove the data annotations from the entity and instead use a special configuration type:

![](domain-layer-2.png)

![](domain-layer-3.png)

::: good  
![Good Example: Domain is lean, configuration for entity is contained within a separate configuration type](domain-layer-3.png)  
:::

This is a big improvement! Now the customer entity is lean, and the configuration can be added to the persistence layer, completely separate of the domain. Now the domain is independent of data access concerns.

Learn more about this approach by reading about [self-contained configuration for code first](https://docs.microsoft.com/en-us/ef/core/what-is-new/ef-core-2.0/#self-contained-type-configuration-for-code-first).

![Figure: Database implementation is a Infrastructure concern not a Domain concern](CA_Animation_4.gif)
