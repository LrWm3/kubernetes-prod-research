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

## General Notes

For notes I have as I am doing other research

- [The Serverless Framework](https://github.com/serverless/serverless) isn't kubernetes driven, but it is interesting. If there was a way to take this and apply it to an on-prem platform, it would be very interesting.
- [Kubernetes performance testing](https://docs.openfaas.com/architecture/performance/) is laid out clearly by the OpenFaaS team.
- [WeaveWorks](https://www.weave.works/product/cloud/) appears to be what I want in terms of visualization

## FAAS

FAAS is something in particular I've always been quite interested in. I found a list of FAAS [in this article](https://blog.alexellis.io/introducing-functions-as-a-service/), at the end of section 2.

### FAAS Features I'm looking for

Here are the features I'm most interested in for now:

- [ ] Opensource
- [ ] Popular, production adoption
- [ ] Still being supported
- [ ] Low overhead, supports up to 1000 activations per second
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
- [ ] External Logging supported

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

- [x] Opensource
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

From the worlds' largest open-source software foundation, [apache](https://apache.org/) has built a [FAAS platform OpenWhisk](https://github.com/apache/incubator-openwhisk). Apache 2.0 License,

### OpenWhisk Features

My evaluation criteria

- [x] Opensource
- [x] Popular, production adoption
  - 4118 Stars, 797 Forks
- [x] Still being supported
  - 12 closed issues, 8 new issues between June 24, 2019 – July 24, 2019
- [ ] Low overhead, supports up to 1000 activations per second
  - [OpenWhisk Triggers per minute is 60](https://github.com/apache/incubator-openwhisk/blob/master/docs/reference.md#triggers-per-minute-fixed-60), so this doesn't have the performance required for a high-throughput FAAS platform
- [x] Documentation, kubernetes documentation in particular
  - [Kubernetes Documentation](https://github.com/apache/incubator-openwhisk-deploy-kube/blob/master/README.md)
- [ ] ~~Tightly coupled with kubernetes, would like to leverage kubernetes CRDs and avoid yet another CLI tool if I could~~
- [ ] ~~Helm deployments would be a plus~~
- [ ] ~~Well-defined CICD pipeline~~
- [ ] ~~UI for managing functions~~
- [ ] ~~UI for visualizing functions networking (can be third-party repo)~~
- [x] Message-bus support ootb, kafka in particular
  - [Kafka is built into openwhisk directly](https://github.com/apache/incubator-openwhisk/blob/master/docs/about.md#please-form-a-line-kafka)
- [ ] ~~Works with encrypted-kafka message-bus~~
- [ ] ~~Fast spin-up time (if any)~~
- [ ] ~~Supports go, java, python and javascript~~
- [ ] ~~Integrates well with logging and monitoring tools~~
- [ ] ~~Roll-out support~~
- [ ] ~~Solid support for End-Users~~
- [ ] ~~Solid support for Developers~~
- [ ] ~~Solid support for Operators~~
- [ ] ~~External Logging supported~~

I stopped filling out this section when I saw the performance limitations. I don't love the idea of standing up an entire docker container each time an activation is run.

### OpenWhisk Licence

[Apache License v2.0](https://github.com/apache/incubator-openwhisk/blob/master/LICENSE.txt)

### OpenWhisk Final Thoughts

Well supported, but its performance limits its usefulness in a FAAS centric development environment

### Kubeless

[Kubeless](https://github.com/kubeless/kubeless) is a kubernetes native FAAS framework. It's not supported by any famous teams though.

### Kubeless License

[Apache License v2.0](https://github.com/kubeless/kubeless/blob/master/LICENSE)

### Kubeless Features

My evaluation criteria

- [x] Opensource
- [x] Popular, production adoption
  - 4890 stars, 500 forks
- [x] Still being supported, not affiliated with any organization
  - 6 closed issues, 10 new issues from June 24, 2019 – July 24, 2019
- [x] Low overhead, supports up to 1000 activations per second
  - [0.24ms / exec over 10 exec with golang](https://medium.com/bitnami-perspectives/introducing-golang-functions-to-kubeless-a9a9e4f0cab1#7828) for a simple 'isPrime' method, 9.0ms / exec with python. Even at 10x slower, with scaling, this can be resolved. This in the right ballpark for number of executions. [Kubeless leverages kubernetes-auto-scaling](https://github.com/kubeless/kubeless#), so `inf` scaling is possible without a gut-busting amount of resources.
  - 
- [x] [Documentation](https://kubeless.io/docs/), kubernetes documentation in particular
  - Yes, also with the [serverless framework](https://github.com/serverless/serverless-kubeless) if iaas providers are an option
- [x] Tightly coupled with kubernetes, would like to leverage kubernetes CRDs and avoid yet another CLI tool if I could
  - [Uses CRD to deploy functions](https://github.com/kubeless/kubeless#), making it about as coupled to kubernetes as possible
  - Does have a [CLI](https://github.com/kubeless/kubeless/releases) though
- [ ] Helm deployments would be a plus
  - Not helm, but one of three [kubernetes](https://kubeless.io/docs/quick-start/) yamls for RBAC & non-RBAC clusters... close enough
- [x] Well-defined CICD pipeline
- [x] [UI for managing functions](https://github.com/kubeless/kubeless-ui)
  - very basic UI, but does allow for editing of at least python functions
- [ ] UI for visualizing functions networking (can be third-party repo)
  - Would require additional research
- [x] [Kafka Message-bus support](https://kubeless.io/docs/pubsub-functions/)
- [ ] Works with encrypted-kafka message-bus
- [x] [Fast spin-up time](https://medium.com/bitnami-perspectives/introducing-golang-functions-to-kubeless-a9a9e4f0cab1#7828) (if any)
- [x] [Supports go, java, python and javascript](https://kubeless.io/docs/runtimes/)
- [x] Integrates well with logging and monitoring tools
  - Not sure about logging, I can see logs are in the pods for functions, but I'm not sure about collecting them
  - [Grafana integration well documented](https://kubeless.io/docs/monitoring/)
- [ ] Roll-out support
  - I'm not sure, may require reading docs more closely
- [x] Solid support for End-Users
- [x] Solid support for Developers
  - Has debugging in cluster, uses kubernetes already, devs are writing only functions
  - Functions can use [configmaps](https://kubeless.io/docs/function-controller-configuration/)
- [ ] Solid support for Operators
  - Not really, its a kubernetes thing
- [x] Solid support for EngIT
  - I'd need to look more closely, but my gut says... YES
  - [Supports multi-tenancy](https://kubeless.io/docs/function-controller-configuration/)

Other notes: Does not currently support distributed tracing, although [there are plans for this in the roadmap](https://github.com/kubeless/kubeless#roadmap)

### Kubeless Final Thoughts

Very promising FAAS project. Meets all my basic requirements; would require additional research to integrate logging, but otherwise looks solid.

### Fission

Fission describes itself as 'Fast Serverless Functions for Kubernetes'

### Fission License

TODO

### Fission Features

TODO My evaluation criteria

- [x] Opensource
- [x] Popular, production adoption
  - 4497 Stars, 405 Forks
- [x] Still being supported
  - 21 closed issues, 11 new issues over June 24, 2019 – July 24, 2019
- [ ] Low overhead, supports up to 1000 activations per second
  - Fast cold spin-up time, [100ms cold-start](https://github.com/fission/fission#performance-100msec-cold-start)
  - Unknown warm spin-up time, couldn't find out from docs
  - Maybe? its whole thing is being fast so I'd hope so.
- [x] [Documentation](https://docs.fission.io/), [kubernetes documentation](https://docs.fission.io/installation/kubernetessetup/) in particular
- [x] Tightly coupled with kubernetes, would like to leverage kubernetes CRDs and avoid yet another CLI tool if I could
  - [Has its own CLI](https://docs.fission.io/installation/)
  - Is definitely tightly coupled with kubernetes
- [ ] Helm deployments would be a plus
- [ ] Well-defined CICD pipeline
- [ ] UI for managing functions
- [ ] UI for visualizing functions networking (can be third-party repo)
- [x] [Kafka Message-bus support](https://docs.fission.io/usage/kafka-trigger-tutorial/)
- [ ] Works with encrypted-kafka message-bus
- [x] Supports [java](https://docs.fission.io/languages/java/) and [go, node, python](https://docs.fission.io/tutorial/enabling-istio-on-fission/)
- [x] [Integrates well with logging and monitoring tools](https://docs.fission.io/tutorial/enabling-istio-on-fission/)
  - Istio integration support
- [x] Roll-out support
- [x] Solid support for End-Users
- [x] Solid support for Developers
- [x] Solid support for Operators
- [x] External Logging supported

### Fission Extra Notes

- Can use [configmaps and secrets](https://docs.fission.io/usage/access-secret-cfgmap-in-function/)
- Working on [fission-workflows](https://github.com/fission/fission-workflows/blob/master/README.md), a product for setting up fission FAAS in DAG. Not sure if there's a UI for it or not yet, but its exactly what I was looking for in terms of organization of FAAS components.

[Fission-workflows](https://github.com/fission/fission-workflows/blob/master/README.md) looks so sick

- Supports `selectors` and `data-transformers` out of the box. `data-transformers` decouple code application input from upstream services. `selectors` allow for filtering on upstream data as well, without unnecessary networking operations.

If a function was written as well which only takes data and inserts it into a database, (kafka-connect, basically), then it would be possible to see flows coming in from rest-apis, going between services and then going out to databases. The only thing which would be missing is if functions were performing REST calls on other functions...?

I just realized, in a function only environment, a function calling another functions' REST api leaves the DAG, I think... But maybe there should be no request-response at all... 

Does seem like databases need to be captured in this type of environment though, since function operations will want to work off of a database. Reinventing 'request-response' is definitely a mistake, however if the DAG can support request response, then things start to make a bit more sense I think.

Check out the [Roadmap for fisson-workflows](https://github.com/fission/fission-workflows/blob/master/Docs/roadmap.md) to really get excited. Unfortunately, the excitement is soon tapered by the [github pulse](https://github.com/fission/fission-workflows/pulse) for the project

### Fission Final Thoughts

Coolest so far, love the `fission-workflows` idea. If it makes room for Request Response & database operations, it would be a full platform for `faas`. Database writes can be done through kafka, but RR probably shouldn't be.

### FaaS-netes / OpenFaaS

[FaaS-netes](https://github.com/openfaas/faas-netes), is OpenFaaS on kubernetes.

### FaaS-netes License

[MIT License](https://github.com/openfaas/faas-netes/blob/master/LICENSE)

### FaaS-netes Features

My evaluation criteria

- [x] Opensource
- [x] Popular, production adoption
  - OpenFAAS has 14,975 stars, FaaS-netes has 1,361
- [x] Still being supported
  - 13 closed, 6 new on faas between June 24, 2019 – July 24, 2019
  - 9 closed, 6 new on faas-netes between June 24, 2019 – July 24, 2019
- [x] [Documentation](https://docs.openfaas.com/), kubernetes documentation in particular ([video](https://youtu.be/0DbrLsUvaso))
- [x] Tightly coupled with kubernetes, would like to leverage kubernetes CRDs and avoid yet another CLI tool if I could, ([has k8 CRD operator](https://github.com/openfaas-incubator/openfaas-operator/))
- [x] [Helm deployments](https://github.com/openfaas/faas-netes#faas-netes) would be a plus
- [x] [Well-defined CICD pipeline](https://docs.openfaas.com/openfaas-cloud/self-hosted/gitlab/)
- [x] [UI for managing functions](https://github.com/openfaas/faas#begin-the-testdrive)
- [ ] UI for visualizing functions networking (can be third-party repo)
- [x] [Kafka Message-bus support](https://github.com/openfaas-incubator/kafka-connector)
- [ ] Works with encrypted-kafka message-bus
- [ ] Fast spin-up time (if any)
- [x] [Supports go, java, python and javascript](https://github.com/openfaas/faas-netes#faas-netes) (supports everything)
- [x] Integrates well with logging and monitoring tools
- [ ] Roll-out support
- [ ] Solid support for End-Users
- [ ] Solid support for Developers
- [ ] Solid support for Operators
- [x] [Solid support for EngIT](https://docs.openfaas.com/)
  - [Self-hosted gitlab integration](https://docs.openfaas.com/openfaas-cloud/self-hosted/gitlab/)
  - [Production](https://docs.openfaas.com/architecture/production/), [Performance](https://docs.openfaas.com/architecture/performance/), [Metrics](https://docs.openfaas.com/architecture/metrics/), [Autoscaling](https://docs.openfaas.com/architecture/autoscaling/), and [Gateways](https://docs.openfaas.com/architecture/gateway/) clearly documented
- [ ] External Logging supported

### FaaS-netes Final Thoughts

Clearly the best supported of all projects I've reviewed so far.

### FaaS-netes Final Thoughts

TODO
