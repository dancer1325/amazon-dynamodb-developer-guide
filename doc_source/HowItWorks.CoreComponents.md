# Core components of Amazon DynamoDB<a name="HowItWorks.CoreComponents"></a>

* Amazon DynamoDB core components = tables + items + attributes
  * tables
    * == collection (WITHOUT limit) of items == bucket
    * data modification events | tables -- can be captured via -- DynamoDB Streams
    * uses
      * store securely your data
    * ⭐primary key MANDATORY to be defined ⭐
    * settings / table
      * capacity
      * backups
      * replications
    * _Example:_ "People" table
  * items
    * == collection of attributes
    * 👁️each item | table -- is identified uniquely by -- primary keys 👁️
      * == 2 items have DIFFERENT primary key
    * primary key
      * 1! / item
      * types
        * simple primary key
          * == ONLY partition key | table
        * composite primary key
          * == partition key + sort key | table
            * -> >= 2 attributes | table
            * if items / SAME partition key ->
              * stored together
              * sort key MUST be different
      * allowed data types
        * `string`
        * `number`
        * `binary`
    * uses
      * store your application data
    * item1's schema (can be) != item2's schema
      * schema == attributes
    * 👁️core unit of data | DynamoDb 👁️
    * types of keys / item
      * partition or hash key
        * uses
          * where data will be stored
        * 's value -- is passed as hash function's input, returning the -- partition == physical storage internal to DynamoDB | store it
        * item1's partition key (can be) != item2's partition key 
      * sort or range key
        * uses
          * way to store items / have SAME partition key == 1toMany
            * -> collection item
    * 👁️ secondary indexes 👁️
      * OPTIONAL 
      * -- belongs to a -- table (named *base table*)
      * allowed >= 1
      * maintained automatically -- sync with -- DynamoDB's items
        * == if you add, update, or delete an item | base table -> DynamoDB adds, updates, or deletes the corresponding item | ANY indexes / -- belong to that -- table
      * allows
        * [querying each item more flexible](SecondaryIndexes.md)
      * types
        * global
          * := index / 's partition & sort key -- can be -- != table's partition & sort key 
          * <= 20 | table
        * local 
          * := index / 's partition key == table's partition & 's sort key != table's sort key
          * <= 5 | table
      * when you create an index -> you specify attributes / will be *projected* -- from the  base table -- to the -- index
        * primary key are MANDATORY projected
    * _Example:_ "person" is an item | table "people"
  * attributes
    * == fundamental data element / NOT broken down
    * == fields or columns | other DDBB
    * nested attributes
      * == attribute's attribute .....
      * <= 32 levels are possible
    * _Example:_ "PersonId" can be n attribute | "person" item

* _Example1:

```
People      // table / schemaless == attributes nor their data types need to be defined beforehand  -> each item can have its own distinct attributes

// next are items       / the primary key is "PersonID"     -> each item is got -- via the -- primary key 
{
    "PersonID": 101,
    "LastName": "Smith",
    "FirstName": "Fred",
    "Phone": "555-4321"
}

{
    "PersonID": 102,
    "LastName": "Jones",
    "FirstName": "Mary",
    "Address": {
                "Street": "123 Main",
                "City": "Anytown",
                "State": "OH",
                "ZIPCode": 12345
    }
}

{
    "PersonID": 103,
    "LastName": "Stephens",
    "FirstName": "Howard",
    "Address": {                        // attribute / have nested attribute
                "Street": "123 Main",
                "City": "London",                                    
                "PostalCode": "ER3 5K8"
    },
    "FavoriteColor": "Blue"             // previous items do NOT have this attribute, because the table is schemaless
}
```

* _Example2:_

    ```
    Music   // table    / schemaless
    
    // next are items       / the primary key is Artist & SongTitle     ==  
    // 1. distinguishes each item | table vs ALL of the others,
    // 2. get each item / providing these keys
    // 3. ways to query items
        // 3.1 Artist
        // 3.2 Artist & SongTitle
    {
        "Artist": "No One You Know",
        "SongTitle": "My Dog Spot",
        "AlbumTitle": "Hey Now",
        "Price": 1.98,
        "Genre": "Country",
        "CriticRating": 8.4
    }
    
    {
        "Artist": "No One You Know",
        "SongTitle": "Somewhere Down The Road",
        "AlbumTitle": "Somewhat Famous",
        "Genre": "Country",
        "CriticRating": 8.4,
        "Year": 1984
    }
    
    {
        "Artist": "The Acme Band",
        "SongTitle": "Still in Love",
        "AlbumTitle": "The Buck Starts Here",
        "Price": 2.47,
        "Genre": "Rock",
        "PromotionInfo": {          // nested attribute
            "RadioStationsPlaying": {
                "KHCR",
                "KQBX",
                "WTNR",
                "WJJH"
            },
            "TourDates": {
                "Seattle": "20150622",
                "Cleveland": "20150630"
            },
            "Rotation": "Heavy"
        }
    }
    
    {
        "Artist": "The Acme Band",
        "SongTitle": "Look Out, World",
        "AlbumTitle": "The Buck Starts Here",
        "Price": 0.99,
        "Genre": "Rock"
    }
    ```

  * if you want to query the data by *Genre* and *AlbumTitle* -> create an index / 
    * partition key -- *Genre*
    * sort key -- *AlbumTitle* 
    * named *GenreAlbumTitle*
    * base table -- *Music*

      | Music Table | *GenreAlbumTitle* | 
      | --- | --- | 
      |  <pre><br />{<br />    "Artist": "No One You Know",<br />    "SongTitle": "My Dog Spot",<br />    "AlbumTitle": "Hey Now",<br />    "Price": 1.98,<br />    "Genre": "Country",<br />    "CriticRating": 8.4<br />}                               <br />                                </pre>  |  <pre><br />{<br />    "Genre": "Country",<br />    "AlbumTitle": "Hey Now",<br />    "Artist": "No One You Know",<br />    "SongTitle": "My Dog Spot"<br />}<br />                                </pre>  | 
      |  <pre><br />{<br />    "Artist": "No One You Know",<br />    "SongTitle": "Somewhere Down The Road",<br />    "AlbumTitle": "Somewhat Famous",<br />    "Genre": "Country",<br />    "CriticRating": 8.4,<br />    "Year": 1984<br />}<br />                                </pre>  |  <pre><br />{<br />    "Genre": "Country",<br />    "AlbumTitle": "Somewhat Famous",<br />    "Artist": "No One You Know",<br />    "SongTitle": "Somewhere Down The Road"<br />}<br />                                </pre>  | 
      |  <pre><br />{<br />    "Artist": "The Acme Band",<br />    "SongTitle": "Still in Love",<br />    "AlbumTitle": "The Buck Starts Here",<br />    "Price": 2.47,<br />    "Genre": "Rock",<br />    "PromotionInfo": {<br />        "RadioStationsPlaying": {<br />            "KHCR",<br />            "KQBX",<br />            "WTNR",<br />            "WJJH"<br />        },<br />        "TourDates": {<br />            "Seattle": "20150622",<br />            "Cleveland": "20150630"<br />        },<br />        "Rotation": "Heavy"<br />    }<br />}<br />                                </pre>  |  <pre><br />{<br />    "Genre": "Rock",<br />    "AlbumTitle": "The Buck Starts Here",<br />    "Artist": "The Acme Band",<br />    "SongTitle": "Still In Love"<br />}<br />                                </pre>  | 
      |  <pre><br />{<br />    "Artist": "The Acme Band",<br />    "SongTitle": "Look Out, World",<br />    "AlbumTitle": "The Buck Starts Here",<br />    "Price": 0.99,<br />    "Genre": "Rock"<br />}<br />                                </pre>  |  <pre><br />{<br />    "Genre": "Rock",<br />    "AlbumTitle": "The Buck Starts Here",<br />    "Artist": "The Acme Band",<br />    "SongTitle": "Look Out, World"<br />}<br />                                </pre>  |

* [restrictions | DynamoDB](ServiceQuotas.md)
* query
  * requirements
    * pass partition key
  * optionally
    * pass sort key
  * != scan
* scan
  * characteristics
    * inefficient
    * expensive

## DynamoDB Streams<a name="HowItWorks.CoreComponents.Streams"></a>

* optional feature
* allows
  * capturing data modification events | DynamoDB tables / 
    * appear
      * in near\-real time
      * in the order / events occurred
    * event -- is represented by a -- *stream record*, if
      * new item is added | table
        * The stream -- captures an -- image of the entire item == ALL its attributes
      * an item is updated
        * The stream -- captures the -- "before" and "after" image of ANY item's attributes / were modified
      * An item is deleted | table
        * The stream -- captures an -- image of the entire item | before delete it
* stream record
  * ALWAYS also contains
    * name of the table
    * event timestamp
    * other metadata
  * lifetime of 24 hours
    * after that -> automatically removed from the stream
* uses
  * \+ AWS Lambda -- to create a -- *trigger*—code / runs automatically if an event of interest appears | stream
    * _Example:_ 
      * *Customers* table / contains customer information for a company
      * goal: send a "welcome" email / each new customer
      * stream | table / stream -- with a -- Lambda function
        * -> once a new stream record appears -> Lambda function would run
        * if new item / has an `EmailAddress` attribute -> Lambda function -- would invoke -- Amazon Simple Email Service / send an email 

    ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/images/HowItWorksStreams.png)
  * data replication | and across AWS Regions
  * materialized views of data | DynamoDB tables
  * data analysis -- via -- Kinesis materialized views
* [Change data capture for DynamoDB Streams](Streams.md)