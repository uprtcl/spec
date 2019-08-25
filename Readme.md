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

The \_Prtcl uses the **internal data schema of Git** to link content together and adapts it to work with ideas instead of code.

With the \_Prtcl, any piece of content can be mutated by anyone, on any platform, while still keeping references to its origin and links to other existing versions of that content. 

Just like GIT, the \_Prtcl encourages divergence and personal perspectives, while facilitating convergence and agreement.

Content that is created with the _Prtcl becomes alive. It can grow and evolve forever, independently of its creator, independently of its platform.

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
  head: Commit;
  context: string;
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

## Structured and Linked Data

The two significant differences between the _Prtcl and GIT is that the _Prtcl works with JSON objects instead of a folder of files, and that the _Prtcl is designed around these objects being linked among them.

The data object can be any JSON object. The _Prtcl does not imposes its structure. 

However, the actions of branching and merging do depend on the data structure. We provide libraries for branching and merging linked/nested objects which will be described later.

One common use case of linking two objects/contexts is to have a soft link from the parent to the child that points to one _perspective_ of the child. Soft links remains the same even if the content of that perspective of the child object changes. 

<p align="center">
  <img src="https://collectiveone-b1.s3.us-east-2.amazonaws.com/Web/uprtcl-linked-objects.png" alt="The Underscore Protocol" width="700" />
</p>
<br/>

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

