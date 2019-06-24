---
description: >-
  We try to stick to these conventions as much as possible. Think of them as
  strong guidelines!
---

# Conventions

## Naming branches in git

Since we using [trunk based development](https://trunkbaseddevelopment.com/) as our workflow, we try to merge code daily and not have any long running "feature branches".

### Daily branches

Daily branches have the developer's initials, the date, and a small title. For e.g.: Asha Kumari making a branch on 12th April to work on "updated search algorithm", should name the branch something like `ak/12apr/update-search-algo`. This gently nudges the developer to merge the branch in a day or two, since this is just a convention and not enforced in any manner.

### Release branches

Release branches have an identifier that it is a _release_ branch, and the date on which the branch is scheduled to go live. For example, the release branch cut on 10th December 2018 will be named `release/2018-12-17` to indicate that it is the release which is scheduled to go live on 17th December 2018.

