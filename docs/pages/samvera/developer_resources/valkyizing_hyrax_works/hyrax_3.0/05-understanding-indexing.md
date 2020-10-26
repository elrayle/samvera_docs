---
title: "Understanding Indexing with Valkyrie"
permalink: valkyrie-work-understanding-indexing.html
sidebar: home_sidebar
keywords: ["Valkyrie", "Work", "Resource"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

## Indexing in ActiveFedora vs. Valkyrie

### ActiveFedora
On save, ActiveFedora writes objects to Fedora and to Solr by calling save on the object.

### Valkyrie

Valkyrie saves resources by passing the resource to a persister.  The persister is configured to write to a single datasource.  At this time, Hyrax only supports the Wings Adapter which converts the resource to an ActiveFedora object and saves it through ActiveFedora.

In the future, Hyrax will support other adapters that write to other datastores (e.g. Postgres).  In this case, when the resource is saved, it is not written to Solr.  This is a separate step.

### Hyrax

Since it is common to want to persist the resource to the primary datastore and write an index document in Solr, there are several processes that support this in Hyrax.

TODO:  NEED TO FLESH THIS OUT
 

<br>
<hr>
<p><a href="valkyrie-work-extra-model-code.html"><button type="button" class="btn btn-primary">Prev: Dealing with extra code in the 2.7 model</button></a> <a href="valkyrie-work-indexer.html"><button type="button" class="btn btn-primary">Next: Creating a Valkyrie Indexer for the Work Resource</button></a></p>
