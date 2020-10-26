---
title: "Custom Change Sets for a Resource Work"
permalink: valkyrie-work-change-sets.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

## Change Sets

To understand Change Sets, see [Dive into Valkyrie tutorial](https://github.com/samvera/valkyrie/wiki/Understanding-change-sets).

## Change Sets and Hyrax

Hyrax automatically creates change sets for new/edit forms.  See <a href="valkyrie-work-new-edit-form.html">New/Edit Forms</a> section for more information.

### Custom Change Sets Use Case

We define 2 work types: PublicationResource and ReleaseResource.  A release can be in one and only one publication.  The publication uses attribute <code>member_ids</code> to link to releases.  The release uses attribute <code>publication_id</code> as a link to the publication.  To support this, we have method <code>ReleaseResource #move_to_new_publication</code> which changes 3 resources: the release, the old publication, and the new publication.

To support this, a change set was defined for publications and releases.

```
class PublicationResourceChangeSet < Hyrax::ChangeSet
  validates :title, presence: true
end
```

```
class ReleaseResourceChangeSet < Hyrax::ChangeSet
  validates :title, presence: true
  validates :publication_id, presence: true
end
```

The basic move process is...

* instantiate a change set for all 3 resources
* in the old publication change set, remove the release id from the old publication's member_ids 
* in the new publication change set, add the release id to the new publication's member_ids 
* in the release change set, set the release's publication id to the new publication id
* validate all 3 change sets<br />Example...
```
    release_change_set.valid?
```
* if validations pass, use UpdateWork transaction to sync each change set and save the resources<br />Example...
```
    Hyrax::Transactions::UpdateWork.new.call(release_change_set).or do |failure|
      raise "Unable to save release #{change_sets[:self_change_set].title}; " \
            "CAUSE: #{failure.first}"
    end
```

<ul class='warning'><li>The <code>UpdateWork</code> transaction calls <code>valid?</code> on the resource being updated, so why do I call <code>valid?</code> before executing the transactions?
<br /><br />
Transactions are not reversible.  When executing multiple transactions, if one passes and a later one fails, this may cause an invalid state.  In an attempt to mitigate this, <code>valid?</code> is called on all 3 resources before calling <code>UpdateWork</code> transaction on each.</li></ul>

<br>
<hr>
<p><a href="valkyrie-work-extra-model-code.html"><button type="button" class="btn btn-primary">Prev: Dealing with extra code in pre-3.0 model</button></a> <a href="valkyrie-work-understanding-indexing.html"><button type="button" class="btn btn-primary">Next: Understanding Indexing with Valkyrie</button></a></p>
