When doing a large change, or one that spans multiple components, it's useful to have a single issue for tracking the overall state of the change. This is called a "meta-issue". It describes the overall change and links to more specific issues for individual pieces.

Meta-issues should all have the special "[meta](https://github.com/dart-lang/sdk/labels/meta)" label. Like other issues, they should be labeled "[Type-Enhancement](https://github.com/dart-lang/sdk/labels/Type-Enhancement)" if they're new features and "[Type-Defect](https://github.com/dart-lang/sdk/labels/Type-Defect)" if they're bugs. If a meta-issue is only relevant to a single project, it should have that project's "Area" label. Otherwise, it should not have an area label.

The initial post of the meta-issue should include a high-level description of the issue followed by a task list for all the sub-issues. Each entry should have a very brief description and a link to the sub-issue. A task list can be created using the following Markdown:

```markdown
- [ ] item
- [ ] item
- [x] checked item
```

Once a sub-issue is completed, its entry should be checked. If applicable, the first release to contain the fix should be mentioned as well.

Occasionally new sub-issues come up. For example, a bug might be found in the solution to a previously-completed sub-issue. To add these to the meta-issue, a triager should both add a comment linking to the new sub-issue *and* add it to the task list. Adding a comment notifies interested parties of a new blocking issue, and updating the task list ensures that the original post continues to provide an up-to-date view of everything required for the meta-issue.

See [this issue](https://github.com/dart-lang/sdk/issues/23454) for a good example of a meta-issue.