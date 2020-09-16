---
title: "Creating a Valkyrie Indexer for the Work Resource"
permalink: valkyrie-work-indexer.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---
 
## Generating solr documents in the Indexer

* Run tests for generated indexer with generated tests -- all tests pass
* Took one change from the ActiveFedora based indexer #generate_solr_document method and added it to new indexer #to_solr method
  * Uncommented #to_solr
  * Add test to check that the custom field was in the solr doc
  * Test passed 
* Copying over remaining #generate_solr_document code to #to_solr
  * Very few and easy changes needed in #to_solr 
  * Add tests to check that the custom fields were in the solr doc
  * Tests passed
* Add specs for indexing defined in publication_metadata.yaml.  
  * Example indexing definition in metadata yaml

```
    index_keys:
      - "status_tesim"
      - "status_sim"
```

  * Tests look like…

```
    context "when indexing defined in metadata yaml" do
      it { is_expected.to have_key(:status_tesim)
      it { is_expected.to have_key(:status_sim)
    end
```

NOTE: Indexing fields defined in metadata yaml are always in the solr_doc even if they do not have a value.  So the tests that fields were created with the expected keys pass without the need to create a publication that has values in every field.

## Writing to the Valkyrie solr core

NOTE: For now, since some models are ActiveFedora::Base objects and others are Valkyrie::Resources, there are two solr cores.  The following describes working with the solr docs created through the valkyrie indexer.

### Setup

Copy valkyrie solr configuration from generators `config/valkyrie_index.yml`.   Also need to set up ENV variables used in the config.

### Calling from tests

For tests requiring that a work be indexed in solr, I use the following setup.

```
let!(:publication) do
  pub = FactoryBox.valkyrie_create(:publicationr, 
                 identifier: ['VALID_IDENTIFIER'])
  solr_doc = PublicationIndexer.new(resource: pub).to_solr
  Hyrax::SolrService.new(use_valkyrie: true)
    .add(solr_doc, commit: true)
  pub
end
```

NOTE: Once fully converted and Hyrax::SolrService is only used with valkyrie core, then you can set config.query_index_from_valkyrie = true in config/initializers/hyrax.rb.  This will allow you to simplify the call to the solr service to…
  
```
Hyrax::SolrService.add(solr_doc, commit: true)  
```

## Calling from application code

TODO: Need to update with more current info.
 
I am currently exploring potential updates to Hyrax to see where it needs to directly run indexer and save the generated solr doc.  I’m looking at transactions which are used when a work form is saved.

If you save a work in your app, change from…

```
af_work.save # save to fedora and AF configured solr core
```

to…

```
Hyrax.persister.save(resource: val_work) # save to datastore only
Hyrax.index_adapter.save(resource: pub)
```

NOTE: The wings valkyrie adapter, which is what Hyrax currently uses, will ultimately call ActiveFedora’s save which will save to Fedora and the AF configured solr core.  The recommendation assumes the end goal is to move to pure Valkyrie with the option to use an adapter other than wings.  Also, custom fields defined in the valkyrie indexer will not be part of the solr doc saved through ActiveFedora.
NOTE: I had issues with Solr complaining about date formats when dates were set in the resource.  I only used this approach when dates weren’t set.  And I had to setup the index_adapter in the test instead of the hyrax.rb initializer.

<br>
<hr>
<p><a href="valkyrie-work-understanding-indexing.html"><button type="button" class="btn btn-primary">Prev: Understanding Indexing with Valkyrie</button></a> <a href="valkyrie-work-custom-queries.html"><button type="button" class="btn btn-primary">Next: Writing Custom Queries</button></a></p>
