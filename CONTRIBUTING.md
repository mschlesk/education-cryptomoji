# Contributing

Thanks for considering contributing to this project! Here are some guidelines
to help speed and unify development.

## Reporting Bugs or Issues

This project uses GitHub issues to track bugs, enhancements, and other tasks.

To create a new Issue:

1. Click on the
[Issues tab](https://github.com/hyperledger/education-cryptobunnies/issues)
underneath the name of this repo above.
2. Click on the
[New Issue button](https://github.com/hyperledger/education-cryptobunnies/issues/new)
on the right side of the page.
3. Enter a title and description for the issue.
4. If applicable, select a label on the right side (such as "bug").
5. Click _"Submit new issue"_.

The title should be a concise label identifying the bug or feature, while the
_"Leave a comment"_ section below should contain a description with as much
detail as possible. For bugs or typos this might include:

- The specific behavior, and what behavior you expected
- What steps someone else could take to reproduce the bug
- Any console logs or other diagnostic information
- Filenames and line numbers if possible (especially for typos)

## Making Code Contributions

Like many projects on GitHub, contributions should be submitted by creating a
fork of the repo, making some changes there, and then submitting them back as a
Pull Request for review. You will find a detailed explanation of this workflow
below. While we are open to any help you'd like to give, if you are looking for
a good place to start, you might checkout the _Issues_ page and see if there is
anything labeled "help wanted" that you could tackle.

### Submitting Pull Requests

While the fork to PR process is common, somewhat more unusual is that `master`
is _not_ the branch you should make a PR against. In this repo, master is not
the source of truth, but a collection of generated stub functions for students
to fill out. The original source of this code is actually in the `staging`
branch, and you should submit _all_ PRs against this branch.

The detailed process is as follows:

1. Create a personal fork of this repository.
2. Starting from `staging`, create a topic branch specific to your changes.
3. Submit a pull request from the topic branch on your fork to `staging` on
   this repo.
4. Add other developers as reviewers.
5. Make changes as requested by rebasing the appropriate commits, and then
   force-pushing to your branch.

Pull requests can be merged once all requests for change have been addressed,
and when approved by at least one project maintainer. The actual merge will be
done by one of the maintainers as well.

Note, maintainers should take care when merging to always use the
_"Create a merge commit"_ option, as rebasing will cause important information
about the commit history to be lost later when the prompt and solution files
are generated.

### Commit Style

Commits should be as small as possible, but no smaller. Ideally, they should be
an "atomic" change to the repo, which add a single new piece of functionality
or fix a single bug, but if broken down any smaller would not provide anything
of value. This is a subjective measure, but use your best judgment.

For the commit message, follow
[Chris Beams's Seven Rules](https://chris.beams.io/posts/git-commit/#seven-rules):

- Separate subject from body with a blank line
- Limit the subject line to 50 characters
- Capitalize the subject line
- Do not end the subject line with a period
- Use the imperative mood in the subject line
- Wrap the body at 72 characters
- Use the body to explain what and why vs. how

In addition to these rules, the commit body should reference the number for the
issue they address. For bug fixes use the syntax: `Fixes #XXX`, and for
enhancements use the syntax: `Closes #XXX`. This information can also go into
the PR message later.

Finally, each commit message must include a _"Signed-off-by"_ line, including
the developer's name and email. This sign-off indicates that you agree the
commit satisfies the
[Developer Certificate of Origin (DCO)](http://developercertificate.org/).
Git can automatically add this signature by using the `-s` flag:

```bash
git commit -s
```

### JavaScript Style

This project follows a slightly modified version of the
[Hack Reactor Style Guide](https://github.com/hackreactor-labs/eslint-config-hackreactor).
There is an `.eslintrc.json` file with these rules in the root project
directory. Any contributor writing JavaScript code should
[install ESLint](https://eslint.org/docs/user-guide/getting-started) and run it
on their code before submitting a PR. The easiest way to do this is to run:

```bash
npm install -g eslint
```

```bash
eslint ./
```

## Prompt Generation Syntax

This repo uses special comment-based tags to differentiate between solution
code and prompts. Later, these tags will be used to separate the prompts into
the `master` branch, and the solution code into the `solution` branch. The
syntax is a slightly more opinionated version of
[problemify](https://github.com/bcmarinacci/problemify). A typical example
might look this:

```javascript
const hello = name => {
  /* START PROBLEM
  // Your code here

  END PROBLEM */
  // START SOLUTION
  console.log(`Hello, ${name}`);
  // END SOLUTION
};
```

With the above stub function, the signature will appear in both the prompts and
the solution code, but the body will be very different:

**master:**
```javascript
const hello = name => {
  // Your code here

};
```

**solution:**
```javascript
const hello = name => {
  console.log(`Hello, ${name}`);
};
```

Note that the comment-tags were removed from _both_ versions. Also note that
the publishing script errs on the side of caution. You must write each tag
_exactly_, on its own line, with the correct style of comment, and in all caps.
Otherwise, it will be ignored. Different indentation is the only supported
variation on the above example.

There is also a syntax to mark an entire file as solution code:
```javascript
/* SOLUTION FILE */
'use strict';

const bar = require('./foo');

// Do some stuff...
```

This entire file would be removed from the `master` branch, only being
published to `solution`. You can also use `/* PROBLEM FILE */` to get the
opposite effect. As with the other tags, make sure to enter it exactly, same
comment syntax, capitalization, on its own line. Unlike the other tags, you may
_not_ indent the file tags, and they _must_ appear on the first line of the
file.

**Examples:**
- [Creating a stub function](https://github.com/hyperledger/education-cryptomoji/blob/master/code/part-two/processor/services/encoding.js#L43)
- [Marking a solution-only file](https://github.com/hyperledger/education-cryptomoji/blob/master/code/part-two/processor/actions/create_collection.js#L1)

### Publishing Updates to `master` and `solution`

Regular contributors need not worry about this, but for maintainers there is a
[bin/publish](bin/publish) script which does the work of parsing the tags and
pushing the generated code to the appropriate branch. Run this script
periodically, preferably after every PR is merged. Make sure your working
directory is clean when you run it, and _never_ do any rebasing or force
pushing to `upstream master` or `upstream solution`. This script pushes to them
directly, but should only ever make additions. If there is a conflict, then
something has gone wrong, and needs to be investigated.
