> Use the following template to provide information about the changes you are introducing to the software. Delete any sections from the template that are not applicable.

Resolves #[issue]

## Summary of changes

> Summarise the changes being introduced:

- xxx
- xxx
- xxx

## User stories

> What user stories do these changes introduce, modify or abandon? Use the following template for each story:

```feature
In order to [do something]
As a [user type]
I want to [some goal]
```

Examples:

```feature
In order to evaluate the performance of the team
As a team manager
I want to generate performance reports
```

```feature
In order to evaluate the effectiveness of my strategy
As a team member
I want to see how my performance compares with my colleagues
```

## Acceptance criteria

> Write some acceptance criteria, covering all possible preconditions, processes and end results. Use the following template for each condition of satisfaction:

```feature
Scenario: [scenario]

Given [state]
 (And [state])
 When [action]
 (And [action])
 Then [result]
 (And [result])
```

Examples:

```feature
Scenario: Team manager views performance report

Given I am logged in as a team manager
 When I go to the Reports section
 Then I see data and charts showing how all team members performed over the last 30 days
  And I can change the report to show results for the last 60 and 90 days

Scenario: Team member views performance report

Given I am logged in as a team member
 When I go to the Reports section
 Then I see data and charts showing my own performance over the last 30 days
  And I can view my results for the last 60 and 90 days too
  And I can see how my performance compares relative to the team average
```

## Checklist

> Please check the relevant boxes `[x]`:

- [ ] The build script produces a production-grade artefact
- [ ] My changes are covered by new tests
- [ ] My tests and all existing tests are passing
- [ ] I have updated the documentation to reflect my changes
- [ ] I have reviewed my coding style against this project's conventions

## Additional comments

> Is there anything else you would like to tell the reviewer?
