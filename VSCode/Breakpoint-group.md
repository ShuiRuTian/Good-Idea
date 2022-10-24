# Breakpoint group

## Motivation
* To enable/disable multi breakpoints quickly

## Features
* A default group
* Add new group with name(Not allow same name)
* Remove existing groups
* Drag and drop
  * drag a break point and drop into another group

## Issue

- one breakpoint in multi groups
  - Maybe introduce "Partially Enabled" state for a group
  - Good
    - flexible
  - Bad
    - How to decide whether one breakpoint is enabled or not? Could one breakpoint have differnt condition if it's in different group?
      - Group could be sorted. The breakpoint is decided by the first group it's in. Support different conditions.
        - Variance: Add some special groups, like "enable group"/"disable group". If a breakpoint is in "disable group" and it's not enabled, the breakpoint is disabled.
      - Add some more control ways. User could choose whether the breakpoint is enabled/disbaled
      - Only one breakpoint is active. User must change the active breakpoint explicitly.

## nested group, could some group manage some other groups?

## API?
