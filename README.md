# PLP Bookstore Database

This MongoDB project sets up a simple bookstore database with a collection of books and demonstrates a variety of database operations, from inserting data to performing advanced queries and optimizations using indexes.

## Database Name
`plp_bookstore`

## Collection
`books`

## Setup Instructions

To get started, open your MongoDB shell and run:

```js
use plp_bookstore;
```

Then insert the sample documents:

```js
db.books.insertMany([...]); // See full data in `books` collection section
```

## Sample Documents

Each book document includes:
- `title` (String)
- `author` (String)
- `genre` (String)
- `published_year` (Number)
- `price` (Decimal)
- `in_stock` (Boolean)
- `pages` (Number)
- `publisher` (String)

> 12 book records are included, featuring authors like George Orwell, J.R.R. Tolkien, and Jane Austen.

## Common Queries

### 1. Find all Fantasy books
```js
db.books.find({ genre: "Fantasy" });
```

### 2. Update the price of *The Alchemist*
```js
db.books.updateOne(
  { title: "The Alchemist" },
  { $set: { price: 15.99 } }
);
```

## Advanced Queries

### Books in stock and published after 2010
```js
db.books.find({
  in_stock: true,
  published_year: { $gt: 2010 }
});
```

### Projection: Only show title, author, and price
```js
db.books.find({}, { title: 1, author: 1, price: 1, _id: 0 });
```

## Aggregation Pipelines

### Average price of books by genre
```js
db.books.aggregate([
  { $group: { _id: "$genre", avgPrice: { $avg: "$price" } } }
]);
```

### Group books by publication decade
```js
db.books.aggregate([
  {
    $group: {
      _id: { $floor: { $divide: ["$published_year", 10] } },
      bookCount: { $sum: 1 }
    }
  },
  {
    $project: {
      decade: { $multiply: ["$_id", 10] },
      bookCount: 1,
      _id: 0
    }
  },
  { $sort: { decade: 1 } }
]);
```

##  Indexing

### Create an index on the `title` field
```js
db.books.createIndex({ title: 1 });
```

### Check query performance before and after using an index
- Without index:
  ```js
  db.books.find({ title: "Jane Austen" }).explain("executionStats");
  ```
- With index:
  ```js
  db.books.find({ title: "Jane Austen" }).hint({ title: 1 }).explain("executionStats");
  ```

## Summary

This project helps you practice:
- Data modeling and insertion
- CRUD operations
- Querying with filters, projections, sorting, and pagination
- Aggregation pipelines
- Index creation and performance testing

