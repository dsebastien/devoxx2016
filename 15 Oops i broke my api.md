# Oops I broke my API

## Types of compatibility
* binary compatibility: a certain type of compiled file can run on both systems without any changes
* source code compatibility: the input file may have to be recompiled for every system, but does not need to be changed before compiling
* behavioral compatibility: the behaviour when processing the input is the same

## Avoid breaking the API when possible
* use internal re-implementations
* use two methods for the same function
* deprecate old methods (redundancy)
  * redundancy can get painful

## Good reasons to break an API
* respond to advances in technology
* for strategic reasons (e.g., stop some unintended usage, reduce abuse, reduce server, load, ...)

## Use semantic versioning
* major minor patch

## A good start design is key
* future-proof designs reduce the need for breaking changes
  * make it modular
  * make it extensible

## Ease the pain
* provide good documentation
* provide conversion tools

## Avoid back-porting when you can
* costs increase over time

## have a clear EOL strategy
* define the EOL for each product version
  * end of support
  * phase out
  * ...

## Communicate early
* don't break "silently"
  * notice before the fact