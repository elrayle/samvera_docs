---
title: "Dealing with extra code in pre-3.0 Active Fedora model"
permalink: valkyrie-work-extra-model-code.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

### Dealing with extra code in Hyrax versions < 3.0

This is specific to our app, but as the patterns may be common, the following are notes related to additional changes.

* Create `app/models/concerns/_MY_RESOURCE_extras.rb` as module PublicationExtras.
* Copy in all code that isn't a property definition

<ul class='info'><li>Did not copy over  <code>include ::Hyrax::WorkBehavior</code>.  The equivalent comes from the class inheriting from <code>Hyrax::Work</code>.</li></ul>

* Removed `self.indexer`.  Valkyrie does not automatically call an indexer when the resource is saved.  See more on this later when indexing is addressed.
* NOT SURE how to convert `include Hyrax::IndexesLinkedMetadata` to Valkyrie equivalent.
* NOT SURE how to convert validations to Valkyrie equivalent.
* Temporarily, include this module in the 3.0 model (e.g. `app/model/_MY_RESOURCE_.rb)

<ul class='info'><li>NOTE: The intention is that all the extra stuff will either be converted to the appropriate Valkyrie equivalent or moved to a service or some other area of the app.  This will leave the resource trim. More on this later.</li></ul>

### Test modifications

* Create a factory for _MY_RESOURCE_
* Copy _ACTIVE_FEDORA_ version of the factory to new _MY_RESOURCE_ factory

<ul class='info'><li>Conversion Tip: My ActiveFedora factory was called <code>:publication</code>.  The new resource factory is called <code>:publication_resource</code>.  My factory contained multiple factories, so the first task was to rename all the factories to append <code>_resource</code> to their names (e.g. <code>factory :publication_resource</code>; <code>factory :ams_publication_resource</code>; etc.)</li></ul>

* update to be similar to Hyrax’s `hyrax_work.rb` resource factory
* setup transient vars for `edit_users`, `edit_groups`, `read_users`, `read_groups`, `members`, and `visibility_setting`
* copy `after(:build)` and `after(:create)` blocks adding in my custom code

<ul class='warning'><li>If you have code in the <code>after(:create)</code> that mutates the resource, you may need to move it to the <code>after(:build)</code> or use <code>Hyrax.query_service</code> to reload the resource in the test after creating it.</li></ul>

* Copy all tests for extra methods from the ActiveFedora model's spec to `spec/models/concerns/_MY_RESOURCE_extras_spec.rb`
* Update references to :_ACTIVE_FEDORA_ factory to :_MY_RESOURCE_ factory

<ul class='info'><li>NOTE: You cannot call create for a factory wrapping a Valkyrie::Resource because resources do not define a #save method.</li></ul>

Options:
* change create to build - This is more efficient anyway and tests should always use build instead of create if it is sufficient for the test.

```
let(:pub) { build(:my_resource) }
```

* use valkyrie_create factory strategy to create the resource

```
let(:pub) { FactoryBot.valkyrie_create(:my_resource) }
```

* If your factory mutates the resource in `after(:create)`, you may need to reload the resource after creating

use build followed by `persister.save` to save the resource before executing tests

``````
let(:pub) do
  pub = FactoryBot.valkyrie_create(:publication_resource)
  Hyrax.query_service.find_by(id: pub.id)
end
```

### Run tests

At this point in the conversion, a lot of tests failed because additional changes were required for the extra code to work with a resource.
  
#### Additional changes made in PublicationExtras module

* Method to get parent collection switched to use

```
Hyrax.custom_queries.find_collections_for(resource: self)
```

* Method to get child works switched to use 

```
Hyrax.custom_queries.find_child_works(resource: self)
```

* I also had to update the factory to set the child works using

```
publication_resource.member_ids = evaluator.members.map(&:id) if evaluator.members
```

* Method to add a publication to a collection switched to use

```
self.member_of_collection_ids += [new_col.id]
```

* Method to remove a publication from a collection switched to use

```
self.member_of_collection_ids -= [old_col.id]
```

### Run tests

All tests of extra code pass except one related to a method to find the publication based on a custom identifier field that holds a slug.  I needed to write a custom query for this that depends on a field value in the solr document.  I’ll come back to this when indexing is working.

<br>
<hr>
<p><a href="valkyrie-work-resource-metadata.html"><button type="button" class="btn btn-primary">Prev: Defining Attribute Metadata for the Work Resource</button></a> <a href="valkyrie-work-understanding-indexing.html"><button type="button" class="btn btn-primary">Next: Understanding Indexing with Valkyrie</button></a></p>
