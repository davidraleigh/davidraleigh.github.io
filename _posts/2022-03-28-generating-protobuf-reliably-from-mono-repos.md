# Generating Protobuf Reliably from a Mono Repo

As a director of software engineering, possibly the most important challenge is how to keep services running properly. Testing can prevent outages and software bugs, but testing and continuous integration must be balanced so that they don't interfere with the productivity of engineers. 

## Why Mono Repos
Testing and integration is easiest with a mono repo. This is especially true if you are running microservices where the microservice messages are evolving. With a mono repo it's easier for an engineer to update a shared library, a client or a service and locally build and test how those changes effect all libraries, clients, or services related to their changes. One of most consequential changes an engineer can make is to change a microservice message.

## Why Protobuf
Google's proto IDL language is concise, strongly type and has a clarity that makes it easy to understand. The Google develope protoc compiler does a great job of generating serialization/deserialization code in most every popular language. The messages themselves are compact on the wire. Protobuf messages are well coupled with the gRPC microservice (where they're the default choise), REST services, or a queue architecture.


JSON and YAML with Swagger can be used to define microservice messages, but it isn't their sole purpose, so it can be a bit messier to use the Swagger IDL to define a service. There are also libraries for generating serialization/deserialization code, but they're still a [work in progress](https://github.com/OpenAPITools/openapi-generator/issues/8892) and don't have the stability of protoc.

## BufBuild
While working on various projects I have relied on Namely's docker containerized protoc for updating serialization/deserialization code. At some point I saw Uber's [prototool](https://github.com/uber/prototool) in the Namely Dockerfile. On a recent peak at prototool I saw that it had been deprecated and pointed to [BufBuild](https://github.com/bufbuild/buf). So if Namely provides a docker containerized protoc what's the use of BufBuild? It helps manage building, linting and checking backwards compatibility of your various proto modules.

### `buf lint`
`buf lint` highlights any naming of proto messages or grpc services that are outside of the defined style guide. This helps keep things pretty and consistent.

### `buf breaking`
`buf breaking` compares the current state of a proto module against a commit of the same proto files to check for breaking changes. Changing of types, removing of messages, renaming of fields and other backwards compatibility failing changes will be flagged.  

### `buf generate`
`buf generate` utilizes a yaml file and a locally installed protoc to generate your protobuf code. This may not sound too different than what protoc does, but actually it prevents the user from having to write a series of bash commands for every language.

## Example Mono Repo with BufBuild and pre-commit
As a demonstration of how a mono-repo can be built using BufBuild and proto I've created a branch, [buf-pre-commit-1](https://github.com/davidraleigh/mono/tree/buf-pre-commit-1), on the [davidraleigh/mono](https://github.com/davidraleigh/mono) github repo. 

A few concepts from [Scripts to Rule Them All](https://github.blog/2015-06-30-scripts-to-rule-them-all/) are used for setting up and testing the project, except that we have a `./scripts/generate.sh` for creating protoc generated code.

In addition to BufBuild I added [pre-commit](https://pre-commit.com/) to try and prevent a bad version of a proto from sneaking into the commit history and allowing for false positives when running the local `buf generate` test in `./scripts/lint/sh` 

This version supports code generation for .Net, C++, Python and Java. Future work will add in Golang. 
