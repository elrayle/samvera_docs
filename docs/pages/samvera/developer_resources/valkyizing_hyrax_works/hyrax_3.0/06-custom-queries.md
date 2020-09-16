---
title: "Writing Custom Queries"
permalink: valkyrie-work-custom-queries.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

## Writing Custom Queries

We had a method that searched solr for a field with a particular value.  This originally used `ActiveFedora::Base #where` method.  It was converted to a [Valkyrie Custom Query](https://github.com/samvera/valkyrie/wiki/Queries#custom-queries) and saved in `app/services/custom_queries/find_by_identifier.rb`.

The custom query was registered in `config/intializers/wings.rb`.

```
require 'wings'
query_registration_target = Valkyrie.config.metadata_adapter.query_service.custom_queries
[CustomQueries::FindPublicationByIdentifier].each do |handler|
  query_registration_target.register_query_handler(handler)
end
```

<br>
<hr>
<p><a href="valkyrie-work-indexer.html"><button type="button" class="btn btn-primary">Prev: Creating a valkyrie indexer for the work resource</button></a> <a href="valkyrie-work-new-edit-form.html"><button type="button" class="btn btn-primary">Next: New and Edit Forms for the Resource Work</button></a></p>
