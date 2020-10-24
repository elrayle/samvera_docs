---
title: "Appendix: Stumbling Blocks"
permalink: valkyrie-work-appendix-stumbling-blocks.html
sidebar: home_sidebar
keywords: ["Issues"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

### roundtrip conversion changes attr's single value into a multi-value array

See [Issue #4556](https://github.com/samvera/hyrax/issues/4556) for more information.

### Changeset fails to save with Ldp:NotFound when adding 2nd of multiple child works one at a time

CAUSED BY: created parent work through valkyrie_create passing in the id for the parent.  

SOLUTION: When the id was removed and the factory generated the id, the error went away.  I did not explore deeper why this happens.  

Seems like you should be able to pass in the id.  

It works when...
* you apply the above solution
* you only add one child
* you add all children by passing in an array of member_ids
* you don't use changesets