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

Selected one of our existing work types (i.e. Publication) and ran the generator for it.  I appended an Resource to the name (i.e. PublicationResource) to avoid clash with existing classes.

```
$ rails generate hyrax:work_resource PublicationResource
  info GENERATING VALKYRIE WORK MODEL: PublicationResource
create app/controllers/hyrax/publication_resources_controller.rb
create config/metadata/publication_resource.yaml
create app/models/publication_resource.rb
create spec/models/publication_resource_spec.rb
create app/forms/publication_resource_form.rb
insert config/initializers/hyrax.rb
create app/indexers/publication_resource_indexer.rb
create app/views/hyrax/publication_resources/_publication_resource.html.erb
create spec/views/publication_resources/_publication_resource.html.erb_spec.rb
```

<ul class='info'><li>You can define attributes with the generator (e.g. attr_name:attr_type) and some of the model related changes in the next section will be generated for you.</li></ul>

This command creates the same work as above adding two attributes authors and issn.

```
$ rails generate hyrax:work_resource PublicationResource authors:string issn:string
```

<br>
<hr>
<p><a href="valkyrie-work-overview-valkyrizing-hyrax-work.html"><button type="button" class="btn btn-primary">Prev: Overview Valkyrizing a Hyrax Work</button></a> <a href="valkyrie-work-resource-metadata.html"><button type="button" class="btn btn-primary">Next: Defining Attribute Metadata for the Work Resource</button></a></p>
