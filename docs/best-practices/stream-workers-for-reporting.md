---
sidebar_position: 110
title: Stream Workers for Reporting
---

When scheduling a reporting job it is common to encounter a large dataset, millions of records or more. Running a query against such a large volume of data is not the most efficient way to create these reports. Instead we can employ a [Stream Worker](/docs/cep/) to aggregate the report data in realtime using complex event processing.

For example, there is a reporting job scheduled at the end of each week on an `ACCESS LOG` collection with millions of records. The report is expected to have records for each day of the week. It is not efficient to run a query on the collection to get the data for all seven days. A `Stream worker` can process data on the `Stream` associated with the collection. This requires the collection stream to be enabled. It can analyze it and generate the `staged` data and can store that data in a `CACHE` collection.

To return the number of the `GET` requests each day from each `IP Address`. Instead of scanning the huge `ACCESS LOG` collection, the `Stream worker` can analyze the data as they are written to the `ACCESS_LOG` collection and store it in the `CACHE` collection. This can include `user information`, the number of `GET` requests, `Date`, `User name` With fewer records compared to the big `ACCESS LOG` collection, the `CACHE` collection can be queried to return the `staged` data.

## Example Stream Worker for Report Processing

```

#################################
#################################
#################################

Example Stream Worker Placeholder

#################################
#################################
#################################

```