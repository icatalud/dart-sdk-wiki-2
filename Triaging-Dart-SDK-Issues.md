##Workflow

* Look through issues that don't have an area assigned.
  * Use the [SDK triage query].
* Does the issue relate to code in the SDK?
  * Assign to the right area by adding a `Area-*` label.
* Is the issue in `Area-Library`?
  * Assign the right `Library-*` label, too.
* Is it obvious if the issue is a Defect or Enhancement?
  * Optional: Add `Type-Defect` or `Type-Enhancement` if you can.
* Does the issue relate to code in another `dart-lang` project/package?
  * Move the issue to the right repo by using the [GitHub Issue Mover][].
* Get emails when issues are tagged with labels you care about
  * Use the [Dart SDK email tool].

## Issue Labels

### Priority
* Level of **team urgency**
  * Should be the opinion of team and/or product management
  * Can be affected by the quantity/priority of existing issues - *If everything is P0, nothing is P0*
  * May evolve as other issues are resolved and new issues are opened
* Levels
    * **P0 Severe**: Drop everything and fix it.
        * For dev channel: blocks the release. Valid cherry-pick.
        * For release channel: worthy of a "dot" release
	* **P1 High**: Planned for the in-progress release
	    * Should be aligned with other work to ensure likely completion in current release
  * **P2 Medium**: Important work for later release
Should be done – eventually
  * **P3 Low**: Maybe, someday
    * First candidates to close as "wont fix, not planned, etc"
* When we enter cherry pick season for release X...
    * P0 issues: are the only fixes that will be taken for release X
    * P1-3 issues:
		* Milestone flag should be changed to release X+1 or
		* P1 issues can be changed to P2/3 and milestone flag removed

### Severity
* Level of **customer pain**
	* Should be the opinion of the customer and/or product management, not necessarily the team it's assigned to.
	* Not affected by the quantity/severity of existing issues.
	* May evolve as we get feedback from users or workarounds become feasible. 
* Levels
	* **S0 Severe**: Affects most users and/or a severely affects critical users. User entirely blocked. Workaround impossible or infeasible.
	* **S1 High**: Affects a lot of users and/or major issues for critical users. Workaround possible, but annoying.
	* **S2 Medium**: Frequent request. A clear annoyance/wart. Straightforward workaround.
	* **S3 Low**: Infrequent request. Debatable annoyance.


[SDK triage query]: https://dart-sdk-email.appspot.com/triage
[GitHub Issue Mover]: https://github-issue-mover.appspot.com/
[Dart SDK email tool]: https://dart-sdk-email.appspot.com/