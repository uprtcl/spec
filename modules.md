## Repositories: all begin with **@uprtcl/**

```mermaid
graph BT
  subgraph dependencies
  common-->orchestrator
  common-->core
  lenses-->core
  end
  subgraph modules
  documents-->common
  documents-->lenses
  tasks-->common
  tasks-->lenses
  chat-->lenses
  end
```

- Micro-orchestrator: UI management layer
  - StoreModule
  - ReduxModule
  - ComponentsModule
- @uprtcl/core: not UI
  - Services: multi, cached, sources, providers, connection, discovery
  - Pattern registry, default and uprtcl patterns
  - Utils
- @uprtcl/common: UI
  - MicroModules: UprtclModule, DiscoveryModule, SourcesModule
  - WebComponents: uprctl-toolbar, uprtcl-history
  - Uprtcl redux
- @uprtcl/lenses: UI lenses management
  - Lenses pattern
  - Providers
  - WebComponents: lens-renderer?
  - Lenses redux
- @uprtcl/documents: UI to deal with text-nodes
  - Redux module
  - Sources module
  - Providers
  - WebComponents: text-node?
- @uprtcl/tasks: UI to deal with tasks
  - Redux module
  - Sources module
  - Providers
  - WebComponents: kanban-board?
- @uprtcl/chat: UI to deal with chats
  - Redux module
  - Sources module
  - Providers
  - WebComponents: chats?

## Runtime instances

```mermaid
graph LR
  subgraph patterns
  PatternRegistry
  end
  subgraph redux
  UprtclRedux-->StoreModule
  LensesRedux-->PatternRegistry
  LensesRedux-->StoreModule
  ChatRedux-->UprtclRedux
  DocumentsRedux-->UprtclRedux
  TasksRedux-->UprtclRedux
  end
  subgraph sources
  DiscoveryModule-->PatternRegistry
  UprtclSources-->DiscoveryModule
  DocumentsSources-->DiscoveryModule
  TasksSources-->DiscoveryModule
  ChatSources-->DiscoveryModule
  end

  subgraph ui
  LensesElements-->LensesRedux
  UprtclElements-->UprtclRedux
  TasksElements-->UprtclElements
  TasksElements-->TasksRedux
  DocumentsElements-->UprtclElements
  DocumentsElements-->DocumentsRedux
  ChatElements-->UprtclElements
  ChatElements-->ChatRedux
  TasksElements-->UprtclElements
  TasksElements-->TasksRedux
  end

```
