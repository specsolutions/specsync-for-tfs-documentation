# Synchronizing Scenario Outlines

Scenario Outlines can be used to specify business rules that can be represented with a table of input-output pairs. The following example shows a scenario outline for specifying the addition function of a calculator.

```text
Scenario Outline: Add two numbers
    Given I have entered <a> into the calculator
    And I have entered <b> into the calculator
    When I press add
    Then the result should be <result> on the screen

Examples: 
    | case          | a  | b  | result |
    | classic       | 50 | 70 | 120    |
    | commutativity | 70 | 50 | 120    |
    | zero          | 0  | 42 | 42     |
```

This scenario outline represents three executable tests, one for each row of the `Examples` table.

In Azure DevOps, parametrized test cases can be created for the similar problems. A test case automatically becomes parametrized, once you use a parameter \(e.g. `@myparam`\) in the test case steps. The parameter values can be specified in a separate table, similarly to scenario outlines: one row in the parameter values table represents one test, called iteration.

SpecSync synchronizes scenario outlines automatically to parametrized test cases. Once the scenario outline above is synchronized, it produces a test case like this.

![Test case synchronized from scenario outline](../../.gitbook/assets/scenario-outlines-parametrized-test-case.png)

## Associating automation for a scenario outline test case \(for SpecFlow\)

For synchronizing automated test cases from Scenario Outline, the test case has to be associated with a single test method \(normally SpecFlow generates one method for each example within the Scenario Outline\). For SpecFlow unit test providers that do not generate a single method, like MsTest, this can be done using the SpecFlow plugin provided by SpecSync. See more about this at [Synchronizing automated test cases](../../important-concepts/synchronizing-automated-test-cases.md).

