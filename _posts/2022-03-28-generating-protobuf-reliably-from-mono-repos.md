# Generating Protobuf Reliably from a Mono Repo

As a director of software engineering, possibly the most important challenge is how to keep services running properly. Testing can prevent outages and software bugs, but testing and continuous integration must be balanced so that they don't interfere with the productivity of engineers. 

## Why Mono Repos?
Testing and integration is easiest with a mono repo. This is especially true if you are running microservices where the microservice messages are evolving. With a mono repo it's easier for an engineer to update a shared library, a client or a service and locally build and test how those changes effect all libraries, clients, or services related to their changes. One of the most consequential changes an engineer can make is to change a microservice message, so having that change contained within one merge request to one repo can make it easier to prevent a breaking change from sneaking through continuous integration.

## Why Protocol Buffers?
Google's [proto IDL language](https://developers.google.com/protocol-buffers) is concise, strongly typed and has a clarity that makes it easy to understand. The Google developed proto compiler, the [protoc](https://developers.google.com/protocol-buffers/docs/overview#cross-lang), does a great job of generating serialization/deserialization code in many popular programming languages. The protocol buffer messages themselves are compact on the wire. These protobuf messages are well suited to gRPC microservices (where they're the [default choice](https://grpc.io/docs/what-is-grpc/core-concepts/#service-definition)), to REST services, or to a queue architecture.


JSON and YAML with Swagger, an alternative to proto, can be used to define microservice messages. But defining strict message formats aren't the sole purpose of JSON or YAML, so it can be a bit messier to use them with the Swagger IDL to define messages. There are also libraries for generating serialization/deserialization code for Swagger messages, but these compilers are still a [work in progress](https://github.com/OpenAPITools/openapi-generator/issues/8892) and don't have the stability of protoc.

If you're interested in learning more about gRPC and Protocol Buffers check out [this talk](https://docs.google.com/presentation/d/1x9s_Kti24HubafpT5gfHGb2xqW6e6SUhbPc6caL0YXU/edit?usp=sharing) I gave at FOSS4G Tanzania.

## BufBuild
While working on various projects I have relied on [Namely's docker](https://github.com/namely/docker-protoc) containerized protoc for updating serialization/deserialization code (for a great summary of gRPC and protobuf checkout out this [Namely talk](https://www.youtube.com/watch?v=RoXT_Rkg8LA)). At some point I noticed Uber's [prototool](https://github.com/uber/prototool) in the Namely Dockerfile, and after a recent peek at the Uber prototool repo I saw that it had been deprecated and pointed to [BufBuild](https://github.com/bufbuild/buf). 

So if Namely provides a containerized protoc what's the use of BufBuild? It helps manage building, linting and checking backwards compatibility of your various proto modules. It means less bash scripts in your toolchain and more automated checks on how your microservice messages evolve. 

### buf lint
`buf lint` highlights any naming of proto messages or grpc services that are outside of the defined style guide. This helps keep things pretty and consistent. There are lots of options you can override if your organization has a different aesthetic (or if you want to adopt BufBuild, but you're not ready to drastically change your protos)

### buf breaking
`buf breaking` compares the current state of a proto module against the last commit of the same proto files to check for breaking changes. Changing of types, removing of messages, renaming of fields and other backwards compatibility failing changes will be flagged.  

### buf generate
`buf generate` utilizes a yaml file and a locally installed protoc to generate your protobuf code. This may not sound too different from what protoc does, but actually it prevents the user from having to write a series of bash commands for every language.

## Example Mono Repo with BufBuild and pre-commit
As a demonstration of how a mono-repo can be built using BufBuild and proto I've created a branch, [buf-pre-commit-1](https://github.com/davidraleigh/mono/tree/buf-pre-commit-1), on the [davidraleigh/mono](https://github.com/davidraleigh/mono) github repo. 

A few concepts from [Scripts to Rule Them All](https://github.blog/2015-06-30-scripts-to-rule-them-all/) are used for setting up and testing the project, except that `./scripts/generate.sh` is added for creating protoc generated code.

In addition to BufBuild I added [pre-commit](https://pre-commit.com/) to try and prevent a bad version of a proto from sneaking into the commit history and allowing for false positives when running the local `buf generate` test in `./scripts/lint.sh` 

This version supports code generation for .Net, C++, Python and Java. Future work will add in Golang. 

