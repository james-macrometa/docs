---
sidebar_position: 90
title: SEARCH Array Attributes
---
To [`FILTER`](/docs/queryworkers/c8ql/operations/filter) attributes in an array of values you would commonly use the array comparison operators, `ALL`, `ANY`, or `NOT`, as a prefix in conjunction with the common comparison operator `IN`. However, this is not an optimized approach and will not utilize any indexes. Users can create [`SEARCH VIEW`](/docs/search/views/create-search-views) for these queries.

The optimized approach uses the `SEARCH` feature. An index is created on the attributes defined in the search view. You can read more about `SEARCH` and search views here, [SEARCH](https://macrometa.com/docs/search/search).

```sql
/* Query on a collection with FILTER */

LET carFeatures = ["AWD", "ABS", "TURBO"]
   FOR car in cars
       FILTER car.features ANY IN carFeatures
   RETURN car

/* Query on Search view with SEARCH */
/* Search VIEW is created with the required attributes. */

LET carFeatures = ["AWD", "ABS", "TURBO"]
   FOR car in CARS_VIEW
     SEARCH ANALYZER((car.features IN carFeatures), "identity")
	 RETURN car
```