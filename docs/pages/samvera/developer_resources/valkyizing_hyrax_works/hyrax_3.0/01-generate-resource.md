---
title: "Generating a Work Resource"
permalink: valkyrie-work-generate-resource.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

## Run generator to create all files for a 3.0 work

Selected one of our existing work types (i.e. Publication) and ran the generator for it.  I appended an r to the name (i.e. Publicationr) to avoid clash with existing classes.

```
$ rails generate hyrax:work_resource Publicationr
  info GENERATING VALKYRIE WORK MODEL: Publicationr
create app/controllers/hyrax/publicationrs_controller.rb
create config/metadata/publicationr.yaml
create app/models/publicationr.rb
create spec/models/publicationr_spec.rb
create app/forms/publicationr_form.rb
insert config/initializers/hyrax.rb
create app/indexers/publicationr_indexer.rb
create app/views/hyrax/publicationrs/_publicationr.html.erb
create spec/views/publicationrs/_publicationr.html.erb_spec.rb
```

<ul class='info'><li>You can define attributes with the generator (e.g. attr_name:attr_type) and some of the model related changes in the next section will be generated for you.</li></ul>

This command creates the same work as above adding two attributes authors and issn.

```
$ rails generate hyrax:work_resource Publicationr authors:string issn:string
```

<br>
<hr>
<p><a href="valkyrie-work-overview-valkyrizing-hyrax-work.html"><button type="button" class="btn btn-primary">Prev: Overview Valkyrizing a Hyrax Work</button></a> <a href="valkyrie-work-resource-metadata.html"><button type="button" class="btn btn-primary">Next: Defining Attribute Metadata for the Work Resource</button></a></p>
