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
----------------------------------------------------------------------
                           Running Benchmarks
----------------------------------------------------------------------
--------------------- CREATING                    TOTAL        AVERAGE
10 Create Publications (ActiveFedora)            66.67ms        6.67ms
10 Create PublicationResources (Valkyrie)         0.39ms        0.04ms
--------------------- SAVING
10 Save Publications with ActiveFedora        6,829.80ms      682.98ms
10 Save PublicationResources with wings      58,012.67ms    5,801.27ms
10 Save PublicationResources in memory            4.96ms        0.50ms
10 Save PublicationResources with postgres      169.54ms       16.95ms
--------------------- READING
10 Read Publications with ActiveFedora          290.67ms       29.07ms
10 Read PublicationResources with wings       3,858.72ms      385.87ms
10 Read PublicationResources from memory          0.07ms        0.01ms
10 Read PublicationResources from postgres       15.42ms        1.54ms
```

<br />
The commands being run for the performance tests are:

_NOTE: Publication is an ActiveFedora::Base model_

```
--------------------- CREATING
af_pub1        = Publication.new(title: ["af pub 1"])
wings_val_pub1 = PublicationResource.new(title: ["wings val pub 1"])
mem_val_pub1   = PublicationResource.new(title: ["memory val pub 1"])
pg_val_pub1    = PublicationResource.new(title: ["postgres val pub 1"])

--------------------- SAVING
af_pub1.save
wings_id = Hyrax.persister.save(resource: wings_val_pub1).id
mem_id   = memory_persister.save(resource: mem_val_pub1).id
pg_id    = postgres_persister.save(resource: pg_val_pub1).id
 
--------------------- READING
ActiveFedora::Base.find(af_pub1.id)
Hyrax.query_service.find_by(id: wings_id)
memory_query_service.find_by(id: mem_id)
postgres_query_service.find_by(id: pg_id)
```

## Exploring ActiveFedora save vs. Hyrax.persister.save

Detailed profiling results are available in...
* [ActiveFedora save]({{site.url}}/profile_af_save.html)
* [Hyrax.persister.save]({{site.url}}/profile_val_save_wings.html)
