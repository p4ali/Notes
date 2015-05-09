**Test-driven development**, or TDD, is an AGILE methodology that flips the development
lifecycle by ensuring that tests are written first, before the code is implemented, and that
tests drive the development (and are not just used as a validation tool).

* **The tenets of TDD are simple:**
 * Code is written only when there is a failing test that requires the code to pass
 * The bare minimum amount of code is written to ensure that the test passes
 * Duplication is removed at every step
 * Once all tests are passing, the next failing test is added for the next required functionality.

* **These simple rules ensure that:**
 * Your code develops organically, and that every line of code written is purposeful.
 * Your code remains highly modular, cohesive, and reusable (as you need to be able to test it).
 * You provide a comprehensive array of tests to prevent future breakages and bugs.
 * The tests also act as specification, and thus documentation, for future needs and changes.

## Behavior Driven Development(BDD)
BDD is an agile process designed to keep the docus on stakeholder value through the whole proejct. The premise of BDD is that the reuquirement has to be written in a way that everyone understands it - business representative, analyst, developer, tester, manager, etc.
The key of BDD is to have a unique set of artifacts that are understood and used by everyone. The artifact here is **BDD story**. A BDD story is written by the whole team and used as both requirements and executable test cases. It's a way to perform test-driven developent with a clarity that cannot be accomplished with unit testing. It's a way to describe and test functionaly in (almost) natural language.

### BDD Story Format
BDD story template have two common elements: **narrative** and **scenario**. The BDD story format looks like this
```
Narrative:
  In order to [benefit]
  As a [role]
  I want to [feature]
  
Scenario: [description]
  Given [context of precondition]
  When [event of action]
  Then [outcome validation]
```
The story is incomplete until discussions about the narrative and one or more scenarios are written.
A narrative is a short, simple descritpion of a feature told from the perspective of a person of role that requires the new functionality. The intention is NOT to provider a complete descrition of what is to be developed but provide a basis for commmunication between all interested parties (business, analysists, developers, testers, etc)
Scenarios descrive the interaction between user roles and the system. Scenarios are the acceptance criteria of the narrative. They represents the definition of done. Once all scenarios have been implemented, the story is considered finished. 
The initial set of senarios should cover the "happy path". Alternative paths should be added progressively during hte duration of the sprint.

### Distinction between traditional requirement and narrative
The difference are precision and planning.
* narrative is more precise
* narratives are better prioirtized, and planned
* reuirement focus on system operations, narratives focus on customer value. 
* narrative encourage looseness of information in order to foster a higer lovel of collabortion between stakeholders and the team.
* The actural work being done is accomplished through collaboration revolving around the narrative that becomes more detailed through scenarios as the development progress.

## Good story
A good BDD narrative use the "INVEST" model:
* Independent. Reduced dependencies = easier to plan
* Negotiable. Details added via collaboration
* Valuable. Provides value to the customer
* Estimable. Too big or too vague - not estimable
* Small. Can be done in less thatn a week by the team
* Testable. Good acceptance criteria defined as scenarios


## Test Types
From low level to high:
* Unit testing - test unit function
* Integration testing - test how object work with other
* Acceptance testing - test usability from stakeholders point of view
* Other
 * Security testing
 * Performance testing
 * Load and stress testing

## Reference
[Behavior driven development through collaboration](http://technologyconversations.com/2013/11/14/behavior-driven-development-bdd-value-through-collaboration-part-1-introduction/)
