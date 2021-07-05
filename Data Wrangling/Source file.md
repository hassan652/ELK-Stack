## Set up data within Elasticsearch
Often times, the dataset is not optimal for running requests in its original state. 

For example, the data type of a field has may not be recognized by Elasticsearch or the dataset may contain a value that was accidentally included in the wrong field and etc. 

These are exact problems that I ran into while working with this dataset. The following are the requests that I sent to yield the results shared during the workshop. 

Copy and paste these requests into the Kibana console(Dev Tools) and run these requests in the order shown below. 

**STEP 1: Create a new index(ecommerce_data) with the following mapping.** 
```
PUT ecommerce_data
{
  "mappings": {
    "properties": {
      "Country": {
        "type": "keyword"
      },
      "CustomerID": {
        "type": "long"
      },
      "Description": {
        "type": "text"
      },
      "InvoiceDate": {
        "type": "date",
        "format": "M/d/yyyy H:m"
      },
      "InvoiceNo": {
        "type": "keyword"
      },
      "Quantity": {
        "type": "long"
      },
      "StockCode": {
        "type": "keyword"
      },
      "UnitPrice": {
        "type": "double"
      }
    }
  }
}
```   
**STEP 2: Reindex the data from original index(source) to the one you just created(destination).**
```
POST _reindex
{
  "source": {
    "index": "name of your original index when you added the data to Elasticsearch"
  },
  "dest": {
    "index": "ecommerce_data"
  }
}
```
**STEP 3: Remove negative values from the unit_price field.**

When you explore the minimum unit price in this dataset, you will see that the minimum unit price value is -11062.06. To keep our data simple, I used the delete_by_query API to remove all unit prices less than 0. 

```
POST ecommerce_data/_delete_by_query
{
  "query": {
    "range": {
      "UnitPrice": {
        "lte": 0
      }
    }
  }
}
```

**STEP 4: Remove values greater than 500 from the unit_price field.**

When you explore the maximum unit price in this dataset, you will see that the maximum unit price value is 38,970. When the data is manually examined, the majority of the unit prices are less than 500. The max value of 38,970 would skew the average. To simplify our demo, I used the delete_by_query API to remove all unit prices greater than 500.
```
POST ecommerce_data/_delete_by_query
{
  "query": {
    "range": {
      "UnitPrice": {
        "gte": 500
      }
    }
  }
}
```