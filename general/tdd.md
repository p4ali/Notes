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

** Imperative and Declarative Approach
* The imperative style of programming focuses on describing individual steps leading to a desired outcome.
* The declarative approach focus on describing the desired result. The individual steps taken
  to reach to the result are taken care of by a supporting framework. It's like saying "Dear
  AngularJS, here is how I want my UI to look when the model ends up witn a certain state. Now
  please go and figure out when and how to repaint the UI". 
