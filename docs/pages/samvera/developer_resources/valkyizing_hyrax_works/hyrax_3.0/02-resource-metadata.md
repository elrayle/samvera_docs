---
title: "Defining Attribute Metadata for the Work Resource"
permalink: valkyrie-work-resource-metadata.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

## Add or Update schema metadata definitions

In ActiveFedora defined works, metadata was defined in property statements in the Work class.  Valkyrie resources use attributes statements in a similar way.  You could manually create attribute statements in your work, but there is a much better way to define metadata in Hyrax 3.0

In Hyrax Valkyrie Resources, the main way to define metadata is in a schema for each resource class.  Schemas are defined in `/config/metadata/_WORK_RESOURCE_CLASSNAME_.yaml`.  The structure of this file includes `attributes` as the first level key.

The second level keys define each attribute's name.  Under each attribute, define its characteristics including the following...

* **type** - data type. Conversion tip: I made the arbitrary decision to set all ours to strings.
* **multiple** - single vs. multiple value. Conversion tip: Set this to the same value as `multiple` in the ActiveFedora property definition.
* **index_keys** - names of the fields in the solr document.  Conversion tip: These equate to the `index.as` settings in the 2.7 property definitions
  * append '_tesim'  to metadata attribute name for :stored_searchable (e.g. "authors_tesim")
  * append '_sim' to metadata attribute name for :facetable (e.g. "authors_sim")
  * see more extension examples at [Solrizer Reference](https://confluence.cornell.edu/display/~elr37/Solrizer+Reference#SolrizerReference-DynamicFieldNamesgeneratedusingSolrizer). _NOTE: Some of this information is dates since Solrizer is no longer in use, but the table with Extensions based on data type and indexed as should be accurate_. 
* **forms** - controls how fields will show up on the new/edit form
  * set required - Conversion tip: Set this to true if the attribute was set in self.required_fields defined in existing Hyrax::_WORK_NAME_Form class; otherwise, false
  * set primary: Conversion tip: Set this to true if you want it to appear above the fold (i.e. when form is loaded and user has not yet pressed more button).  In our case, if field is required, I set primary to true; otherwise, false
  * multiple - can the user enter multiple values through the form.  Conversion tip: This is generally the same as the main level value of multiple.  So this was again set to the value of multiple in the ActiveFedora property definition.        

Edit `config/metadata/_WORK_RESOURCE_CLASSNAME_.yaml` to define the work resource metadata.

<ul class='info'><li>Any attributes defined with the generator will have a basic entry generated in the metadata schema file.</li></ul>

For every property statement in 2.7 Publication model (`app/model/_ACTIVE_FEDORA_WORK_MODEL_.rb`), define an entry in the schema following the pattern...

```
   authors:
    type: string
    multiple: true
    index_keys:
      - "authors_tesim"
      - "authors_sim"
    form:
      required: false
      primary: false
      multiple: true
    # predicate: ::RDF::Vocab::MARCRelators.aut
```
See [basic_metadata.yaml](https://github.com/samvera/hyrax/blob/master/config/metadata/basic_metadata.yaml) for a complete example

<ul class='warning'><li>Predicate - NOT SUPPORTED - I included the predicate to avoid losing this information.  At this time, it is not used by Hyrax-Wings, but hoping that there will be some type of implementation for predicates in the future.  To avoid YML.safe_load errors, the predicate is commented out.</li></ul>
  
## Write tests for Work Resource Metadata

<ul class='info'><li>Any attributes defined with the generator will have the basic respond_to test generated in the model spec.</li></ul>

Edit `spec/models/publicationr_spec.rb` and add tests for attributes.  

For every property statement in 2.7 Publication model (app/model/publication.rb), define a respond_to test…
```
it { is_expected.to respond_to(:authors) }
```

<br>
<hr>
<p><a href="valkyrie-work-generate-resource.html"><button type="button" class="btn btn-primary">Prev: Generating a Work Resource</button></a> <a href="valkyrie-work-extra-model-code.html"><button type="button" class="btn btn-primary">Next: Dealing with extra code in the 2.7 model</button></a></p>
