## Getting your changes to dev channel during cherry pick season

We batch up cherry picks and do pushes a number of times a week. if you have a cl that you think should make it to the current release, follow these steps.

First, please validate that your cl merges cleanly and that relevant tests pass:

```
git new-branch --upstream origin/dev merge_my_awesome
git cherry-pick #HASH_FROM_MASTER_THAT_NEEDS_CHERRY_PICKING
```

Then, build and run the tests. If it does not merge cleanly please supply a patch by fixing merge conflicts locally and uploading a cl to rietveld by doing git cl upload, link to the review from the merge request.

Then, please file a [merge request issue](https://github.com/dart-lang/sdk/issues/new?title=Please%20merge%20revision%20%23HASH%20into%20dev%20channel&body=@ricowind%20@whesse%20@kasperl%0A%3CFill%20in%20the%20labels%20for%20Milestone%20and%20Area%20to%20indicate%20what%20the%20merge%20applies%20to.%20E.g.%20if%20you%27re%20requesting%20a%20merge%20to%20fix%20an%20issue%20in%20Dart2JS,%20set%20the%20area%20to%20Area-Dart2JS.%20If%20this%20is%20a%20request%20to%20merge%20to%20stable%20please%20change%20the%20MergeToDev%20label%20to%20MergeToStable%3E%0A%0A%3CDescribe%20the%20problem%20this%20merge%20is%20fixing%20-%20include%20issue%20numbers%20if%20applicable%3E%0AEXAMLE:%20This%20fixes%20the%20promote%20script%20to%20not%20assume%20integer%20revisions%20after%20our%20github%20move%0A%0A%3CWhat%20revision(s)%20needs%20to%20be%20merged%20-%20please%20annotate%20revisions%20with%20reason%20if%20more%20than%20one%3E%0AEXAMLE:%206030dd9015dc11208d630c1739763f448e5332d7%0A%0A%3CPlease%20try%20out%20the%20merge%20yourself.%20If%20there%20are%20merge%20conflicts%20please%20resolve%20these%20and%20upload%20the%20cl%20to%20rietveld%20and%20provide%20a%20link%20here%3E%0AEXAMPLE:%20This%20merged%20cleanly&labels=MergeToDev)