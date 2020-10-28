---
title: "Appendix: Performance"
permalink: valkyrie-work-appendix-performance.html
sidebar: home_sidebar
keywords: ["Performance"]
last_updated:
version:
  id: hyrax_3.0-stable
  label: 'Hyrax v3.0'
---

## Summary of Performance Times

The times in the table are rough wall clock times. <br />

  TOTAL = the sum of 10 runs of a command <br /> 
  AVERAGE = TOTAL / 10 

```
----------------------------------------------------------------
                      Running Benchmarks
----------------------------------------------------------------
--------------------- CREATING              TOTAL        AVERAGE
1 Create Publications for ActiveFedora    369.74ms      369.74ms
1 Create Publications for wings            20.58ms       20.58ms
1 Create Publications for memory           23.09ms       23.09ms
1 Create Publications for postgres         25.45ms       25.45ms
--------------------- SAVING
1 Save Publications with ActiveFedora   4,286.73ms    4,286.73ms
1 Save Publications with wings          9,152.73ms    9,152.73ms
1 Save Publications in memory              80.20ms       80.20ms
1 Save Publications with postgres         675.49ms      675.49ms
--------------------- READING
1 Read Publications with ActiveFedora     289.14ms      289.14ms
1 Read Publications with wings          1,024.95ms    1,024.95ms
1 Read Publications in memory               4.89ms        4.89ms
1 Read Publications with postgres         155.30ms      155.30ms
```

The commands being run for the performance tests are:
```
--------------------- CREATING
Publication.new(title: ["pub #{idx}"]) }
PublicationResource.new(title: ["pubr #{idx}"]) }
PublicationResource.new(title: ["pubr #{idx}"]) }
PublicationResource.new(title: ["pubr #{idx}"]) }

--------------------- SAVING
publications_for_active_fedora[idx].save }
Hyrax.persister.save(resource: publication_resources_for_wings[idx]) }
memory_persister.save(resource: publication_resources_for_memory[idx]) }
postgres_persister.save(resource: publication_resources_for_postgres[idx]) }
 
--------------------- READING
ActiveFedora::Base.find(publications_for_active_fedora[idx].id) }
Hyrax.query_service.find_by(id: publication_resources_for_wings[idx].id) }
memory_query_service.find_by(id: publication_resources_for_memory[idx].id) }
postgres_query_service.find_by(id: publication_resources_for_postgres[idx].id) }
```

## Exploring ActiveFedora save vs. Hyrax.persister.save

Detailed profiling results are available in...
* [ActiveFedora save]({{site.url}}/profile_af_save.html)
* [Hyrax.persister.save]({{site.url}}/profile_val_save_wings.html)
