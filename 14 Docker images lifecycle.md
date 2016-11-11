# Docker images lifecycle

## Goal

Define the lifecycle for Docker images

## Analogy
Not really like Debian packages: app + declaration of dependencies to resolve
Instead, close to War files

## Quality gates & visibility
Each staging area is isolated from the other ones by a quality gate.
* first quality gate: should i commit my code
* CI
* CI ok? ...

## CI/CD pipeline
...

## Versions and dependencies
Be careful to make sure you have the same docker containers in various environments
* FROM ubuntu:14.04: not precise enough
* FROM ubuntu:<sha2> of the base image
  * requires additional documentation to precise which version that is
* also anything you put in the images!

## Traditional server pattern
* package server image
* provision server image
* apply configuration change
* again
* ...

## Immutable server pattern
* package server image
* provision server image
* change required
  * package new server image
  * ...

## Immutability with Docker
* not only use a container, but also make sure it's the same container all the way

## Example
* docker pull nginx
* add jdk 8
* jfrog xray
* add input from npm
* test build
* promote