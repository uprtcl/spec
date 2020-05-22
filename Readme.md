<p align="center">
  <a href="https://www.uprtcl.io">
    <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/sticker1.png" alt="The Underscore Protocol" width="400" />
  </a>
</p>

<p align="center">
  <a href="https://github.com/uprtcl/js-uprtcl"><img src="https://img.shields.io/badge/Frontend%20Infrastructure-yellow.svg?style=flat-square" /></a> <a href="https://demo.uprtcl.io"><img src="https://img.shields.io/badge/Demo-Text Editor-yellow.svg?style=flat-square" /></a>
  
  
</p>

<p align="center">
  <a href="https://github.com/uprtcl/eth-uprtcl"><img src="https://img.shields.io/badge/%20Provider-Ethereum-brightgreen.svg?style=flat-square" /></a>
  <a href="https://github.com/uprtcl/hc-uprtcl"><img src="https://img.shields.io/badge/%20Provider-Holochain-yellow.svg?style=flat-square" /></a>
  <a href="https://github.com/uprtcl/js-uprtcl-server"><img src="https://img.shields.io/badge/%20Provider-Spring%20Boot-brightgreen.svg?style=flat-square" /></a>
</p>

<br/>

# The Underscore Protocol (_Prtcl)

The Underscore Protocol (_Prtcl) combines the core ideas behind The World Wide Web and GIT into the concept of Evolving Entities, or “Evees”.

An Evee is a platform-agnostic object that:

- Like web URLs, it can be linked with other Evees, on other apps, on any platform.
  
- Like GIT repositories, it can have multiple “perspectives”: forks and mutations created and evolved by anyone, on any platform.

- It can be reused, rendered and interacted with on different apps and platforms.

Read more about our implementation of the protocol in our [documentation site](https://uprtcl.github.io/).

## Why _Prtcl

The Web has evolved significantly since its original vision of linked hypertext documents. Instead of *web pages*, most of The Web is now made of *web applications*. 

Web applications are usually made of a frontend application and a backend service, and instead of working with HTML pages and URLs, users interact with the frontend by consuming, creating or interacting with **data objects** that are, ultimately, read and stored from *entries* in a backend database.

_Prtcl is a way to have, with data objects, what early Web users had with HTML documents and URLs: the possibility to link, render, reference and interact with data objects of any application from any *other* application. 

To achieve this, frontend applications and backend service should start handling and rendering Evees (Evolving Entities) instead of ephemeral data objects.

## What is _Prtcl

_Prtcl is built on top of a few main pillars: Evees, the Cortex Framework, and Service Providers.

### Evees

Evees are like GIT repositories but their content is a JSON object, instead of a set of folders and files. Evees evolve as a sequence of content-addressable *Commits*, a sequence that might fork at a given Commit, creating more than one *head*. Each *head* of the sequence is referred to as a *Perspective* of the Evee.

Instead of working with objects, applications should work with *Perspectives* of an Evee. 

Perspectives are important because they enable parallel independent versions of an object to co-exist, instead of forcing all the applications and users of an object to interact with the same version, stepping on each other. They enable freedom and divergence while facilitating agreements and convergence.

Here is a diagram of an Evee, evolving as a sequence of commits, and passing from having one perspective (`zbP1`) to having two (`zbP1` and `zbP2`).

<img src="https://docs.google.com/drawings/d/e/2PACX-1vSlA1MRL3bRrnBaHmtT-QwCPYiyOV0yBntl1-Go2MZaekMfH2SMEBtSEmP380dOonuLTlPqQuCb9Zm0/pub?w=1595&amp;h=796">

### Cortex Framework

Web applications and backend services are, usually, strongly coupled with the data structure of the objects they manipulate and fail at manipulating any other type of object. This prevents Evees from being interoperable among different apps. 

One challenge to achieving the _Prtcl vision is, therefore, to develop applications that can dynamically render and manipulate objects having different data structures. This needs to be achieved, however, without forcing strong restrictions on app developers about the type of data and operations they can perform on their apps.

This is what The Cortex Framework does. Its a generic framework for specifying frontend functionalities around *Entities* (data objects). With Cortex:

- One-object-multiple-apps (OOMA): One type of object is rendered and manipulated in different applications.
- One-app-multiple-objects (OAMO): One application is able to render and manipulate objects of different types. 

Cortex is built around the concept of patterns: a duck-type-like dynamic typing system that associates a given *behavior* to entities that fulfill a given *condition* on its data structure or value.

To learn more about Cortex see the [`js-uprtcl` documentation](https://github.com/uprtcl/js-uprtcl/wiki).

### Service Providers

Service Providers are the combination of a backend service and a frontend javascript class that, together, can perform a standard functionality, specified as an interface, for the rest of frontend modules. The backend service can be a web server, but can also be a decentralized network.

Web applications usually interact with a single backend service, but this will not be the case as applications start working with Evees that can reside on multiple platforms.

The following Evees Service Providers are being developed:

- [js-uprtcl](https://github.com/uprtcl/js-uprtcl): Includes a Local Service Provider, using the native browser indexedDB to store Evees.  
- [eth-uprtcl](https://github.com/uprtcl/eth-uprtcl): Store and govern Evees in Ethereum and IPFS.
- [js-uprtcl-server](https://github.com/uprtcl/js-uprtcl-server): Store and protect access to Evees in a NodeJS web REST API.
- [hc-uprtcl](https://github.com/uprtcl/hc-uprtcl): Store, govern and protect access to Evees in Holochain.


## The _Prtcl stack

The image below shows an example of how two _Prtcl compatible apps are structured (at a high level). 

OOMA: Both apps import the `Evees` and the `Tasks` Cortex Modules and, thus, let their users interact with "Task Evees". Tasks created in one app can be rendered, edited or forked on the other app.

OAMO: App 1 includes the Tasks and Documents modules, whose entities have a few *Patterns* in common (not shown in the figure). This way a Documents and a Task can be rendered together in say, a "list" area.

<img src="https://docs.google.com/drawings/d/e/2PACX-1vT8KcnmE8WMtBON8eJeFWgQXl1exABuKZ3ZD4NOG9-t9CxeRUVCnuKl-6zYb0rKOk6irglHC0WDQOPM/pub?w=1281&amp;h=729">


## _Prtcl Reusable Modules

A set of open-source _Prtcl Cortex modules is being developed. These modules **will soon be released** and will include web-components, state-management, patterns, and service providers to let developers integrate _Prtcl functionalities on their apps in a matter of minutes.

This is a list of the web-components from the modules that (will soon be) available:

- `GenericPost`: Bundles of text and images, similar to a Tweet or a Facebook post.
- `SimpleEditor`: Simple editor to write blog-posts or documents that can scale.
- `KanbanBoard`: Kanban board to organize cards in columns.
- `Calendar`: Calendar board to crate and display events.
- `Drawer`: A drawer to organize and store Evees.

This is how the web components listed above might look like:

<img src="https://docs.google.com/drawings/d/e/2PACX-1vS-cjj7LlU6anl95b5ROe08EKAjqmZXFrrHRn8jrAZM-ycpFFzicOeW6NjRyF9l7u6lGmseeL6p2kWG/pub?w=1977&amp;h=1749">

These modules are especially well suited for web 3.0 applications in which user authentication and data storage is decentralized, but they can be used in web 2.0 applications using a Micro Services backend architecture.

## Where to go from here

To integrate a reusable Cortex module in your app see [`js-uprtcl` documentation](https://github.com/uprtcl/js-uprtcl/wiki).



