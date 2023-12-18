# Service Architecture Project

This repo describes the architecture and design on a service based architecture used to seperate sensitive data from other data.

## Components
- Client: [https://github.com/SHerlihy/auth-frontend](https://github.com/SHerlihy/auth-frontend)
- Mediator: [https://github.com/SHerlihy/auth-mediator-sfg](https://github.com/SHerlihy/auth-mediator-sfg)
- Sensitive Database: [https://github.com/SHerlihy/auth-service-sfg](https://github.com/SHerlihy/auth-service-sfg) [https://github.com/SHerlihy/auth-db](https://github.com/SHerlihy/auth-db)
- Public Database: [https://github.com/SHerlihy/profile-service-rfg](https://github.com/SHerlihy/profile-service-rfg) [https://github.com/SHerlihy/profile-db](https://github.com/SHerlihy/profile-db)

## Current Network Flow

```mermaid
flowchart TD
    CLI[Client]
    Med{Mediator}
    Sens[Sensitive Database]
    Pub[Public Database]

    RevProxy[Reverse Proxy]

    CLI --> Med

    Med --> Sens
    Sens --> Med

    Med --x RevProxy
    RevProxy --> Pub
    Pub --> RevProxy

    RevProxy --> Med

    Med --> CLI
```

```mermaid
sequenceDiagram
    participant cli as Client
    participant med as Mediator
    cli->>med: HTTP

    create participant sens as Sensitive Database
    med->>sens: HTTP
    destroy sens
    sens->>med: HTTP

    create participant rev as Reverse Proxy
    med-xrev: TCP
    create participant pub as Public Database
    rev->>pub: HTTP
    destroy pub
    pub->>rev:HTTP
    destroy rev
    rev->>med:TCP

    med->>cli: HTTP
```
