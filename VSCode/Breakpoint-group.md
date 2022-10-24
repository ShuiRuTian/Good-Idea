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
    - Could one breakpoint have differnt conditions if it's in different group?
    - How to decide whether one breakpoint is enabled or not?
      - Group could be sorted. The breakpoint is decided by the first group it's in.
        - Variance: Add some special groups, like "enable group"/"disable group". By this way, we do not need to sort. For example, we could give "Disable group" highest priority ---- if a breakpoint is in "disable group" and the group is not enabled, then the breakpoint is disabled, no matter whatever other groups is like.
      - Add some more control ways. User could choose whether the breakpoint is enabled/disbaled by the relation between the groups.
      - Only one breakpoint is active. User must change the active breakpoint explicitly.

## nested group, could some group manage some other groups?

## API?
