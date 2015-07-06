When doing a large change, or one that spans multiple components, it's useful to have a single issue for tracking the overall state of the change. This is called a "meta-issue"; it describes the overall change and links to more specific issues for individual pieces.

Meta-issues should all have the special "[meta](https://github.com/dart-lang/sdk/labels/meta)" label. Like other issues, they should be labeled "[Type-Enhancement](https://github.com/dart-lang/sdk/labels/Type-Enhancement)" if they're new features and "[Type-Defect](https://github.com/dart-lang/sdk/labels/Type-Defect)" if they're bugs. If a meta-issue is only relevant to a single project, it should have that project's "Area" label; if it spans multiple projects, it should be labelled "[Area-Multi](https://github.com/dart-lang/sdk/labels/Area-Multi)".

The initial post of the meta-issue should include a high-level description of the issue followed by a checklist for all the sub-issues. Each entry should have a very brief description and a link to the sub-issue. A checklist can be created using the following Markdown:

```markdown
- [ ] item
- [ ] item
- [x] checked item
```

Once a sub-issue is completed, its entry should be checked. If applicable, the first release to contain the fix should be mentioned as well. 

See [this issue](https://github.com/dart-lang/sdk/issues/23454) for a good example of a meta-issue.