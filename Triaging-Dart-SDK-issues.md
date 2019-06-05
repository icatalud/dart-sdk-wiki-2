## Workflow

* Look through issues that don't have an area assigned.
  * Use the [SDK triage query].
* Does the issue relate to code in the SDK?
  * Assign to the right area by adding an `area-*` label. **Note: `area-language` is deprecated. New language issues should be [moved][move] to the [language repo](https://github.com/dart-lang/language).**
* Is the issue in `area-library`?
  * Assign the right `library-*` label, too.
* Is it obvious if the issue is a bug or enhancement?
  * Optional: Add `type-bug` or `type-enhancement` if you can.
* Does the issue relate to code in another `dart-lang` project/package?
  * Move the issue to the right repo by using [`/move to ...`][move].
* Get emails when issues are tagged with labels you care about
  * Use the [Dart SDK email tool].

## Issue Labels

### Priority
* Level of **team urgency**
  * Should be the opinion of team and/or product management
  * Can be affected by the quantity/priority of existing issues - *If everything is P0, nothing is P0*
  * May evolve as other issues are resolved and new issues are opened
* Levels
    * **[p0-critical][]**: Drop everything and fix it.
        * For dev channel: blocks the release. Valid cherry-pick.
        * For release channel: worthy of a "dot" release
	* **[p1-high][]**: Planned for the in-progress release
	    * Should be aligned with other work to ensure likely completion in current release
  * **[p2-medium][]**: Important work for later release.
      * Should be done – eventually.
  * **[p3-low][]**: Maybe, someday
    * First candidates to close as "closed-not-planned"
* When we enter cherry pick season for release X...
    * p0 issues: are the only fixes that will be taken for release X
    * p1-3 issues:
		* Milestone flag should be changed to release X+1 or
		* p1 issues can be changed to p2/3 and milestone flag removed

[SDK triage query]: https://goto.google.com/dart-triage
[move]: https://github.com/apps/move
[Dart SDK email tool]: https://dart-sdk-email.appspot.com/
[p0-critical]: https://github.com/dart-lang/sdk/labels/p0-critical
[p1-high]: https://github.com/dart-lang/sdk/labels/p1-high
[p2-medium]: https://github.com/dart-lang/sdk/labels/p2-medium
[p3-low]: https://github.com/dart-lang/sdk/labels/p3-low