# Module monoliths

## Monolithic architecture
* simpler
* one line code change = redeploy everything

## Microservices
* service-based architecture
* scalabile
* polyglot programming
* complex (operational & monitoring)

## Monoliths are big balls of mud (or... not?)
"If people can't build monoliths propertly, microservices won't help"

"I see you have a poorly structured monolith. Would you like me to convert it into a poorly structured set of microservices?"

"A good structure is easy to visualize"

"Abstraction is about reducing detail, rather than creating a different representation

"Abstractions help us reason about big and complex software systems"

## Architecture description (C4 model)
* context (level 1): system, people using it (roles), external systems it interacts with
* containers (level 2): zoom in on an element of the context diagram. Shows all parts
* components (level 3): zoom in on an element of the containers diagram. Shows lower level details (e.g., interactions with the file system, batch jobs, etc)
* classes/code (level 4): zoom in on an element of the components diagram. Show the internal details of that component

The issue is that components are not necessarily actual things that exist in code. Rather, they're conceptual.

There's a conceptual gap between the code and the model.

## Model-code gap
Your architecture models and your source code will not show the same things.
The difference between them is the model-code gap. Your architecture models include some abstract concepts like "components", that your programming language does not, but could.
Beyond that, architecture models include intensional elements, like design decisions and constraints, that cannot be expressed in procedural source code at all.

Consequently, the relationship between the architecture model and the source code is complicated.
It is mostly a refinement relationship, where the extensional elements in the architecture model are refined into extensional elements in the source code. However, intensional elements are not refined into corresponding elements in the source code.

Upon learning about the model-code gap, your first instinct might be to avoid it. But reflecting on the origins of the gap gives little hope of a general solution in the short term: architecture models help you rason about complexity and scale because they are abstract and intensional; source code executes on machines because it is concrete and extensional.

## Layered architectures in code
People usually separate architectural layers in code (e.g., through packages in Java, maven modules, etc).
Without being careful, it usually ends up as a mess as developers violate the constraints and restrictions between those layers (e.g., misplacing elements, linking layers that should never be, etc).

Should layers be considered harmful?

## Cargo cult programming
Applying something without really knowing the why.

Is architectural layering in software another cargo cult?

Make conscious decisions!

## Complexity & layering
Once an app starts being non-trivial, architectural layers are not enough anymore. We then need to modularize inside the layers.

Applying this can be done by using the package by feature approach (vertical slices). It doesn't solve everything though

=> Modularity as a principle

## Impermeable boundaries
Apply component-based design and service-orientation to a monolithic application

## How to design software
Decomposition strategy: goal is to break down the application in smaller parts to minimize static dependencies among the parts and to maximize the cohesiveness of each part

## Interaction style
* synchronous
* asynchronous
* commands
* messages
* events

## Testing
Do not let tests drive your design!

Options:
* test the service in isolation
* test the repository in isolation
* test the component through its public API

Test your significant structural elements as black boxes

Component testing does not suit asynchronous behavior and integration with third party systems outside of your control

Goal: align the test you write with the code you create

Revisited pyramid of testing:
* system tests: UI, API and acceptance tests (e2e)
* component and service tests: focused on components and services through their public API
* class tests: focused on specific classes and methods, sometimes using mocking

## Public
Stop making every class public.




