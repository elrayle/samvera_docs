---
title: "Dealing with extra code in the 2.7 model"
permalink: valkyrie-work-extra-model-code.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

### Dealing with extra code in 2.7 model

This is specific to our app, but as the patterns may be common, the following are notes related to additional changes.

Create app/models/concerns/publication_extras.rb as module PublicationExtras.
Copy in all code that isn't a property definition
Did not copy over  `include ::Hyrax::WorkBehavior`.  The equivalent comes from the class inheriting from Hyrax::Work
Removed `self.indexer`.  Valkyrie does not automatically call an indexer when the resource is saved.  See more on this later when indexing is addressed.
NOT SURE how to convert `include Hyrax::IndexesLinkedMetadata` to Valkyrie equivalent.
NOT SURE how to convert validations to Valkyrie equivalent.
Temporarily, include this module in the 3.0 model (e.g. app/model/publicationr.rb)

NOTE: The intention is that all the extra stuff will either be converted to the appropriate Valkyrie equivalent or moved to a service or some other area of the app.  This will leave the resource trim. More on this later.
Test modifications
Create a factory for Publicationr
copy Publication factory to new Publicationr factory
the code is basically the same except all the factories need to append an `r` to the end of the factory name (e.g. `factory :publicationr`; `factory :ams_publicationr`; etc.)
update to be similar to Hyrax’s hyrax_work.rb resource factory
setup transient vars for edit_users, edit_groups, read_users, read_groups, members, and visibility_setting
copy `after(:build)` and `after(:create)` blocks adding in my custom code
Copy all tests for extra methods from publication_spec.rb to publication_extras_spec.rb
Update references to :publication factory to :publicationr factory

NOTE: You cannot call create for a factory wrapping a Valkyrie::Resource because resources do not define a #save method.

Options:
•	change create to build - This is more efficient anyway and tests should always use build instead of create if it is sufficient for the test.
let(:pub) { build(:publicationr) }

•	use build followed by `persister.save` to save the resource before executing tests
let(:pub) do
  pub = build(:publicationr)
  Hyrax.persister.save(resource: pub)
end

•	use `FactoryBot.valkyrie_create` to create the resource.  It will do the save for you.  NOTE: If your factory changes the resource in the `after(:create)` block, you will need to reload the resource to pick up the change.
# no changes in after(:create) so don’t need to reload
let(:pub){ FactoryBot.valkyrie_create(:publicationr) }
 
# after(:create) puts the new pub in waob collection,
# so need to reload to see the relationship
let(:pub2) do
  pub = FactoryBot.valkyrie_create(:waob_publicationr)
  Hyrax.query_service.find_by(id: pub.id)
end


Run tests
A lot of tests fail because additional changes are required for the extra code.  
Additional changes made in PublicationExtras module
Method to get parent collection switched to use
Hyrax.custom_queries.find_collections_for(resource: self)

Method to get child works switched to use 
Hyrax.custom_queries.find_child_works(resource: self)
I also had to update the factory to set the child works using
publication.member_ids = evaluator.members.map(&:id) if evaluator.members

Method to add a publication to a collection switched to use
self.member_of_collection_ids += [new_col.id]

Method to remove a publication from a collection switched to use
self.member_of_collection_ids -= [old_col.id]

Run tests
All tests of extra code pass except one related to a method to find the publication based on a custom identifier field that holds a slug.  I need to write a custom query for this.  I’ll come back to this when indexing is working as it depends on the identifier field being indexed.

<br>
<hr>
<p><a href="valkyrie-work-resource-metadata.html"><button type="button" class="btn btn-primary">Prev: Defining Attribute Metadata for the Work Resource</button></a> <a href="valkyrie-work-understanding-indexing.html"><button type="button" class="btn btn-primary">Next: Understanding Indexing with Valkyrie</button></a></p>
