# Screenplay Next generation automated acceptance testing

## Sustainable test automation
* high cost maintenance
  * fragility and difficulty to maintain

Goal: shift from verifying implementation back to helping building great user experiences.

"44-80% of all defects are caused by unclear, ambiguous and incorrect requirements". (actual studies)

## Testing approach: hierarchical task analysis
From user-centered design. Focus on the actor (user) rather than on the browser.
Focus on what the user tries to achieve. To achieve its goal, he has to perform a number of tasks which consist in a number of interactions with the system.

* Actor -> Goals -> Tasks -> Interactions
* Example
  * User -> Record things to do -> Record an item -> Type "walk the dog", Press enter

Most tests start at the interaction level (sequences of interactions).

## BDD scenarios
Example
* Feature: filter the list to find items of interest
  * in order to focus on outstanding items -> James <- would like to filter his todo list to only show items of interest
  * Scenario: Viewing active items only
  * Given -> James <- has a list with -> Walk the dog, Get a coffie <- -> AND <- he completes -> Walk the dog <-
  * When he filters his list to show only -> Active <- tasks
  * Then his todo list should contain -> Get a coffie <-

## Screenplay pattern
Rather than expressing your tests in terms of interactions with the UI you express your terms in of what the business user/actor is trying to achieve.

Business tasks vs interactions.

Example tasks:
* Scenario: Viewing active items only
* Given James has a list with Walk the dog, Get a coffie
* And he completes Walk the dog
* When he filters his list to show only Active tasks
* Then his todo list should contain Get a coffie

Lower level version:
* Scenario: Viewing active items only
* Start with a list containing Walk the dog, Get a coffie
* Complete a todo item called: Walk the dog
* Filter list to show only active items
* Expect to see: Get a coffie

This lower level version allows us to see tasks as composites of lower level actions. For example
* Task: Start with a list containing Walk the dog, Get a coffie
  * Open the browser on '...'
  * Resize browser window to maximum
  * Add a todo item called "Walk the dog"
    * Enter the value "Get a coffie"
    * Hit the enter key
  * Add a todo item called "Get a coffie"
  * ...

This approach allow for a lot of code reuse because there are much larger number of scenarios than tasks.

## Testing code quality
"40-70% of maintenance overhead is due to poorly written test suites"

Software craftsmanship matters a lot in tests.

## Serenity BDD and the Screenplay pattern
There is a Java and a JavaScript version.

```
Actor james = Actor.named("James");
```

Actors can have "abilities" that allow them to interact with the system:

```
@Managed
WebDriver hisBrowser;

james.can(BrowseTheWeb.with(hisBrowser));
```

Actors perform tasks:
```
james.attemptsTo(AddATodoItem.called("Get a coffie"))
```

Those building blocks have a clear meaning and can easily be composed, even by junior developers. Business users or analysts can also read and understand this.

Step definition example:
```
@Given("^.* has a todo list containing (.*)$")
public void hasAListWith(List<String> items) {
  james.attemptsTo(
    Start.withATodoListContaining(items);
  );
}
```

What allows to scale the tests is creating a sort of business-domain specific DSL.

Tasks can use other tasks:
```
public class Start implements Task {
  @Step("{0} starts with **#todoListDescription")
  public <T extends Actor> void performAs(T actor) {
    actor.attemptsTo(
        Open.browserOn().the(applicationHomepage),
        AddTodoItems.called(items)
    );
  }
}
```

Interactions use lower level constructs:
```
public class AddATodoItem implements Task {
  @Step("{0} adds a todo item called: #thingToDo")
  public void performAs(Actor theActor) {
    theActor.attemptsTo(
      Enter.theValue(thingToDo),
        .into(WHAT_NEEDS_TO_BE_DONE)
        .thenHit(RETURN)
    );
  }
}
```

## Visibility
Test reports does not give enough visibility.

It's important for good communication to automate the execution of the test suites so that trust can be built by showing we are building working software implementing the requirements.

* high level: release readiness
* capabilities
* features
* scenarios
* steps

## Pending scenarios
Scenarios can be marked as pending, to state that they're not done yet. This is useful to see in reports that some test scenarios have not been finished/implemented yet, while not breaking the build.
