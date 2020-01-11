---
name: Feature
about: Suggest functional changes
title: ''
labels: feature
assignees: ''
---

Use the following template to provide information about the changes in functionality you propose to introduce to the software. Delete any sections from the template that are not applicable.


## User Stories
Write one or more user stories that describe the proposed feature and its benefits from the perspective of particular groups of users. Use the following template for each story:

```
In order to [do something]
As a [user type]
I want to [some goal]
```

Examples:

```
In order to evaluate the performance of the team
As a team manager
I want to generate performance reports

In order to evaluate the effectiveness of my strategy
As a team member
I want to see how my performance compares with my colleagues
```


## Acceptance Criteria
Provide a list of acceptance criteria, covering all possible preconditions, processes and end results. Use the following template for each condition of satisfaction:

```
Scenario: [scenario]

Given [state]
 (And [state])
 When [action]
 (And [action])
 Then [result]
 (And [result])
```

Examples:

```
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


## Contribute?
Are you able to contribute the required source code changes yourself? Check one box `[x]`:
- [ ] Yes
- [ ] No
