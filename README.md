# kubernetes-prod-research

Research into a complete production-ready kubernetes development environment.

## Goals 

Here's everything I can think of in terms of what I'd like my production-ready kubernetes development environment would have.

Indented items are some ideas which would be cool to have.

- RBAC
- Multi-tenancy
  - Virtual Clusters
- FAAS
  - openfaas (- its a service / + its better supported)
- CICD; deploy, build, test
- Easy to deploy
- kubernetes managed ssl-certs for subdomains
- in-cluster ssl
- high-throughput, stateful message-bus
  - kafka
  - kstreams
  - ksql
- cluster-specific volume support
  - nfs
- database adapters
  - postgresql configured with database, table & row level permissions
  - deployable permissioned graphql adapters for postgresql
- cluster-specific helm registry
- Authentication / Authorization tied to LDAP / Active Directory
- security features like whitelists / blacklists / DDOS defense
- all stateful, persisted data can be easily configured to push to PV somewhere?
  - This would require more thought, as kubernetes basically already supports this...
- developer / admin tools
  - kubeapps
  - cluster visualization tools?
  - kubeadmin?
- monitoring and logging tools
  - graphana
  - fluentd
- kafka-connect support, (with persist?)
- Can leverage GPU / support GPU-passthrough / provisioning
- Self-hosted heroku? deviates from the design I had planned, but might be nice for people who want more control over their platform

## FAAS

FAAS is something in particular I've always been quite interested in. I found a list of FAAS [in this article](https://blog.alexellis.io/introducing-functions-as-a-service/), at the end of section 2.

### FAAS Features I'm looking for

Here are the features I'm most interested in for now:

- [ ] Popular, production adoption
- [ ] Still being supported
- [ ] Documentation, kubernetes documentation in particular
- [ ] Tightly coupled with kubernetes, would like to leverage kubernetes CRDs and avoid yet another CLI tool if I could
- [ ] Helm deployments would be a plus
- [ ] Well-defined CICD pipeline
- [ ] UI for managing functions
- [ ] UI for visualizing functions networking (can be third-party repo)
- [ ] Message-bus support ootb, kafka in particular
- [ ] Works with encrypted-kafka message-bus
- [ ] Fast spin-up time (if any)
- [ ] Supports go, java, python and javascript
- [ ] Integrates well with logging and monitoring tools
- [ ] Roll-out support
- [ ] Solid support for End-Users
- [ ] Solid support for Developers
- [ ] Solid support for Operators

I'll go through the list for each FAAS I evaluate from here on out.

### FAAS Features I'm Less interested in

I care less about these things

- Authorization optional, this could be handled at a different layer, but docs on permissioning wouldn't hurt
- Supports any language, if the languages it supports include go, java, python and javscript, in that order of priority
- In-cluster Debugging support

### Funktion

[Funktion](https://github.com/funktionio/funktion) is no longer supported by RH. The README.md suggests OpenWhisk or Kubeless.

I won't bother looking at anything else in this case.

### Iron Functions

From the [iron](https://iron.io/) team, comes [Iron Functions](https://github.com/iron-io/functions). Unfortunately for me, iron is closed source. However, Iron Functions is open-source.

#### Iron Functions' Features

- Documentation covers running on kubernetes
- Made for the iron ecosystem, which is a paid-for closed-source iaas layer meant for on-prem with full GPU support. I don't know if I need this
- Uses an iron-specific CLI Tool, providing separation from kubernetes, but also infrastructure lock-in
- Supports authentication on two levels; one layer is all requests coming into the service, the other layer is all requests on a specific context-path
- UI Supported
- Functions can be written in any language
- Import functions from Amazon Lambda
- Supports message queues 'Bolt' 'Redis' and 'IronMQ'

- [x] Popular, production adoption
  - 2621 Stars, 205 Forks on GitHub
- [ ] Still being supported
  - Roadmap last updated in 2017
- [x] Documentation, kubernetes documentation in particular
- [ ] Tightly coupled with kubernetes, would like to leverage kubernetes CRDs and avoid yet another CLI tool if I could
  - Made for the iron ecosystem, which is a paid-for closed-source iaas layer meant for on-prem with full GPU support. I don't know if I need this
- [ ] ~~Helm deployments would be a plus~~
- [ ] ~~Well-defined CICD pipeline~~
- [ ] ~~UI for managing functions~~
- [ ] ~~UI for visualizing functions networking (can be third-party repo)~~
- [ ] ~~Message-bus support ootb, kafka in particular~~
- [ ] ~~Works with encrypted-kafka message-bus~~
- [ ] ~~Fast spin-up time (if any)~~
- [ ] ~~Supports go, java, python and javascript~~
- [ ] ~~Integrates well with logging and monitoring tools~~
- [ ] ~~Roll-out support~~
- [ ] ~~Solid support for End-Users~~
- [ ] ~~Solid support for Developers~~
- [ ] ~~Solid support for Operators~~

Strike-through I didn't bother evaluating

#### Iron Functions' License

[Apache License v2.0](https://raw.githubusercontent.com/iron-io/functions/master/LICENSE)

#### Iron Functions' Final Thoughts

Ongoing development and support is never optional.

### OpenWhisk

From the worlds' largest open-source software foundation, [apache](https://apache.org/) has built a FAAS platform OpenWhisk. Apache 2.0 License,

### OpenWhisk Features



### OpenWhisk Licence

[Apache License v2.0](https://github.com/apache/incubator-openwhisk/blob/master/LICENSE.txt)

### Kubeless


### Fission


### FaaS-netes

