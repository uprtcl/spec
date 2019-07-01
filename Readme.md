# The Underscore Protocol (_Prtcl)

The _Prtcl uses the **internal data schema of Git** to link content together and adapts it to work with ideas instead of code.

With the _Prtcl, any piece of content can be mutated by anyone, on any platform, while still keeping references to its origin and links to other existing versions of that content. 

Just like GIT, the _Prtcl encourages divergence and personal perspectives, while facilitating convergence and agreement.

Content that is created with the _Prtcl becomes alive. It can grow and evolve forever, independently of its creator, independently of its platform.

## Spec

Creating _prtcl-compatible content is as simple as creating three (3) small support objects/identifiers every time **a brand new piece of content** is created (if its not brand-new, but a mutation, not all tree need to be created).

These are the three support objects:

### Commits

The commit object links a piece of content to its previous version. It needs a link to the content and a link to its parent commit. Commit MUST be content-addressable. Linked commits build a directed acylic graph (DAG).

### Perspectives

Perspectives is how git "branches" are called in the _Prtcl. Perspectives represent independently evolving "versions" of a piece of content.

A perspective is one "named" tip of the DAG of commits. You can create/**branch** new perspectives out of existing ones and you can **merge** changes made on a perpective into another one.

Perspectives are **NOT** content-addressable. The perspective identifier must be the same after its latest commit is updated.

### Contexts

Contexts are how git "repositories" are called in the _Prtcl. On its most basic form, Contexts are just a string that multiple Perspectives use to represent the fact that they are perspectives of the same "thing", the same "context".

## Core Data Schema

The objects below show the minimum structure that _prtcl-compatible objects MUST include. The value of each property is of type `string` or `Array<string>`, but the type listed is an Object name to show that the `string` value MUST be an identifier of an object of *that* type .

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

## Suggested Extended Data Schema

A *suggested* data schema for _prtcl-compatible objects is provided below. It adds metadata to the Commit and Perspective objects that help applications and users work with these objects. It also proposes a standard way to compute the Context identifiers.

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

Because a perspective head must not be content-addressale, the `origin` property provides information about the "source of thrust" which will provide the the latest head commit id of *that* perspective.

 Some examples of how the origin property COULD look are listed below:

- A URL of a webserver API `https://www.collectiveone.org/uprtcl`
- An address of a blockchain smart contract. `eth://0x1a5c29c94D03C4c8f7414564CBD57295d61e898f`
- An IPFS OrbitDb `log` or Textile `thread` identifier that helps deriving the latest head.

In addition, the perspective identifier SHOULD be content-addressable of at least its `origin`, but **without** including its head id. This way the origin of a perspective can be trusted from its id.

Its possible that the perspective metadata (its origin, creator, etc) is stored in a content-addressable platform, while the perspective head is stored in another platform that supports non-content-addressable shared data identifiers.

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

### Context

To prevent context identifiers duplication and increase the value of a context by associating them to a person or entity, context identifiers are built from their author id and the current timestamp.

```
Context {
  creator: string;
  timestamp: number;
  nonce: number;
}
```










