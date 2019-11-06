<p align="center">
  <a href="https://www.uprtcl.io">
    <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/sticker1.png" alt="The Underscore Protocol" width="400" />
  </a>
</p>

<p align="center">
  <a href="https://github.com/uprtcl/web-components"><img src="https://img.shields.io/badge/Frontend-Web%20Components-yellow.svg?style=flat-square" /></a> <a href="https://demo.uprtcl.io"><img src="https://img.shields.io/badge/Demo-Text Editor-yellow.svg?style=flat-square" /></a>
  
  
</p>

<p align="center">
  <a href="https://github.com/uprtcl/eth-uprtcl"><img src="https://img.shields.io/badge/%20Provider-Ethereum-brightgreen.svg?style=flat-square" /></a>
  <a href="https://github.com/uprtcl/hc-uprtcl"><img src="https://img.shields.io/badge/%20Provider-Holochain-yellow.svg?style=flat-square" /></a>
  <a href="https://github.com/uprtcl/java-uprtcl"><img src="https://img.shields.io/badge/%20Provider-Spring%20Boot-brightgreen.svg?style=flat-square" /></a>
</p>

<br/>

# The Underscore Protocol (_Prtcl)

The _Prtcl is a simple set of rules for creating, linking, evolving, branching and merging data objects on different platforms, by different users.

The \_Prtcl uses the **internal data schema of Git** and adapts it to work with ideas (linked data objects) instead of code.

With the \_Prtcl, any object can be mutated by anyone, on any platform, while still keeping references to its origin and links to other existing versions of that content. 

Just like GIT, the \_Prtcl encourages divergence and personal perspectives, while facilitating convergence and agreement.

Objects created with the _Prtcl become alive. They can grow and evolve forever, independently of its creator, independently of its platform.

## Spec

Creating \_prtcl-compatible content is as simple as creating three (3) small support objects/identifiers every time **a brand new piece of content** is created (if its not brand-new, but a mutation, not all tree need to be created).

These are the three support objects:

### Commits

The commit object links a piece of content to its previous version. It needs a link to the content and a link to its parent commit. Commits MUST be content-addressable. Linked commits build a directed acylic graph (DAG).

### Perspectives

Perspectives is how git "branches" are called in the \_Prtcl. Perspectives represent independently evolving "versions" of a piece of content.

A perspective is one "named" tip of the DAG of commits. You can create/**branch** new perspectives out of existing ones and you can **merge** changes made on a perpective into another one.

Perspectives are **NOT** content-addressable. The perspective identifier must be the same after its latest commit is updated.

### Contexts

Contexts are how git "repositories" are called in the \_Prtcl. On its most basic form, Contexts are just a string that multiple Perspectives use to represent the fact that they are perspectives of the same "thing", the same "context".

## Core Data Schema

The objects below show the minimum structure that \_prtcl-compatible objects MUST include. The value of each property is of type `string` or `Array<string>`, but the type listed is an Object name to show that the `string` value MUST be an identifier of an object of *that* type .

### Schema

```
Commit {
  parents: Array<Commit>;
  data: Data;
}
```

```
Perspective {
  origin: string; // inmutable
  head: Commit; // mutable
  context: string; // mutable
}
```

### Diagram 

The figure below shows how the Commit, the Perspective and the Context objects/identifiers relate to each other and to the data object.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/_prtcl-core-objects.png" alt="The Underscore Protocol" width="700" />
</p>
<br/>

## Suggested Extended Data Schema

A *suggested* data schema for \_prtcl-compatible objects is provided below. It adds metadata to the Commit and Perspective objects that help applications and users work with these objects. It also proposes a standard way to compute the Context identifiers.

### Commit

Add author, timestamp and a custom "message" to a commit.

```
Commit {
  creator: string;
  timestamp: number;
  message: string;
  parents: Array<Commit>;
  data: string;
}
```

### Perspective

Add author, timestamp and a custom "name" to a perspective. Add also the `origin` of the Perspective. 

Because a perspective head must not be content-addressale, the `origin` property provides information about the "source of thrust" which will provide the latest head commit id of *that* perspective.

 Some examples of how the origin property COULD look are listed below:

- A URL of a webserver API `https://www.collectiveone.org/uprtcl`
- An address of a blockchain smart contract. `eth://0x1a5c29c94D03C4c8f7414564CBD57295d61e898f`
- An IPFS OrbitDb `log` or Textile `thread` identifier that helps deriving the latest head.

In addition, the perspective identifier SHOULD be content-addressable of at least its `origin`, but **without** including its head id. This way the origin of a perspective can be trusted from its id.

It's possible that the perspective metadata (its origin, creator, etc) is stored in a content-addressable platform, while the perspective head is stored in another platform that supports non-content-addressable shared data identifiers.

This is the extended Perspective object specification.

```
Perspective {
  origin: string;     
  creator: string;
  timestamp: number;
  context: Context;
  name: string; 
  head: Commit;       // excluded for id computation
}
```

## Linked Data

The two significant differences between the _Prtcl and GIT is that the _Prtcl works with JSON objects instead of a folder of files, and that the _Prtcl is designed around these objects being linked together.

One common use case of linking objects/contexts is to have a soft link from the parent to the child that points to one _perspective_ of the child. The figure below shows how two contexts can be linked to one parent context using soft links.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-linked-objects.png" alt="The Underscore Protocol" width="700" />
</p>
<br/>

## Data Structure

> This section is under construction

The _Prtcl does not impose a structure to the JSON objects with which it works. However, some operations, like branching and merging, do depend on the data structure. The _Prtcl tools provide routines that help handle the most common cases, and let developers extend these to fit their own needs.

## Service Providers

> This section is under construction

The _Prtcl is platform-agnostic, meaning that the objects can be stored on any platform, including a web-server, a blockchain or a distributed storage network like IPFS.

## Context Rendering

> This section is under construction

Objects created with the _Prtcl can live on many platforms, be used by many apps, and be modified by many authors. 

In this environment, having common and reusable tools to render contexts on a web-browser is considered a key aspect of the technology.


## Example - A Document

As an example, consider how a simple document would be created and modified using GIT and how this would be different using the _Prtcl. 

In GIT, the document would, most likely, live on a root folder storing also the database of the GIT repository. There would be one text file (for the first paragraph) and two subfolders (one for each subsection), each with its own text file for the content. 

The folder structure in this example would be as follows

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/git-folder-structure.png" width="300" />
</p>

If a user wants to create a modification of the content of Section 1, he needs to branch the entire repository, updating the GIT repository database in the root folder, and then change the contents of the `Sit Ei.txt` file.

This is how it would look:

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/git-doc-branch-example.png" width="700" />
</p>


In the case of the _Prtcl, each element of the document can be a context (and it's own repository), each context having, as data, a `TextNode` object with the following structure:


```
TextNode {
  text: string;
  type: string;
  links: Array<string>;
}
```

The figure below shows the graph of interconnected contexts (a total of six contexts shown in green) that build up the entire document. The sequence of commits for each context (in blue) and the new perspective/branch on the `SIT` context (in yellow) as perspective/branch on that context only, and not on the entire document `DOC1`.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-doc-branch-example.png" width="700" />
</p>
 
The figure below shows the document structure in the  _Prtcl were each element is a virtual context. In this example, the `mutation` perspective (branch) applies only to the first paragraph of Section 1, and not to the entire document.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-doc-branch-example-02.png" width="700" />
</p>

With the _Prtcl, it is now possible for the new perspective on the `SIT` context to be reused on another context, say, for example, another document `DOC2`, without losing the link with its origin within the `DOC1` context. 

This link can be, eventually, used to bring back changes to the `SIT` context made in `DOC2` back into `DOC1`. Note that this link would be lost in the case the `DOC2` had been created as a new GIT repository.

The figure below shows how the `mutation` perspective of the `SIT` context is reused in another context `DOC2` without losing the link with the original document.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-docs-linked-example-02.png" width="700" />
</p>

And the figure below shows how two perspectives of the same contexts can live in two documents `DOC1` and `DOC2` at the same time.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-docs-linked-example.png" width="700" />
</p>

### New Perspective - Recursive Branching

In the _Prtcl, branches are called perspectives and differ nothing with GIT branches: they are pointers to the latest commit (head) of that branch. 

However, creating a new perspective in the _Prtcl is different from creating a new branch in GIT because contexts in the _Prtcl can be linked.

How are links treated while creating a new perspective of a context depends on its data structure and possibly on the user preferences. 

Here we provide an example of how new perspectives of documents could be created using a recursive branching strategy. In this example, the links on the parent context data are updated to point to the new perspectives of the child contexts.

The figure below shows an example of a new global perspective of `DOC1`. Blue tags are the ids of the current perspectives for each context and point to their corresponding head commit (blue circles). 

The global perspective operation creates a new perspective on `DOC1` (with new ids in yellow tags) and all of its subcontexts and adds a commit (yellow circles) to contexts with children (`DOC1`, `S1` and `S2` ), to update their “subcontexts links” to point to the new perspectives of their subcontexts.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-global-perspective.png" width="700" />
</p>

The figure below shows the resulting document after having created a new global perspective of `DOC1`. The content remains the same although new perspectives and links among them have been created.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-global-perspective-02.png" width="700" />
</p>

Note that, in the _Prtcl, perspectives are not identified by their names string as are branches in GIT, but have unique identifiers. More importantly, even if created together, the new perspectives of each of the subcontexts are actually independent of each other, being initially linked by the “subcontext link” committed to the parents, but not bounded to remain together or act together in any way other than that.

In other words, the “yellow” and “blue” colors in the figures above are not actual categories of the data, but simply subjective interpretations that derive from the user experience.

### Merge Two Perspectives - Recursive Context-based Merging

Merges in the _Prtcl treat differently the data of a context than its links to subcontexts, even if both are part of a commit object.

In the case of a `TextNode`, the text property is merged either automatically or manually by accepting/rejecting modifications. 

However, for each link (child element) of the "to" parent `TextNode`, if there is a link to a perspective **of that same context** in the "from" parent `TextNode`, instead of updating the link of the "to" parent to point to the child perspective of the "from" parent, those two child perspectives (of the same context) are merged recursively, leaving the link of the "to" parent untouched.

The figure below shows the state before doing a global merge of `DOC1:c1285` into `DOC1:a231c`. The data set by each commit (that changes the data of a context) is marked with gray tags and a table of the data is provided on the right.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-global-merge-01.png" width="700" />
</p>

The figure below shows the state after having done the global merge of `DOC1:c1285` into `DOC1:a231c`.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-global-merge-02.png" width="700" />
</p>

And the figure below shows the document view both perspectives and the result of their merge.

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-global-merge-03.png" width="700" />
</p>



