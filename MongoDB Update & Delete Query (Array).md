## MongoDB Update Query: Conditionally Adding a New Element to an Array Field

This query demonstrates how to conditionally add a new element to an existing array field in MongoDB. In this example, if the categories array in the expenseType
collection contains elements, we use the __$push__ operator to add a new category while preserving the existing ones.

Here's how you can structure the query to add a new category to categories in the expenseType collection if it already contains other elements:

``` javascript

db.expenseType.updateOne(
  {
    _id: ObjectId("67262c4e9be22fd3ef8d2596"),   // Document identifier
    "categories.0": { $exists: true }            // Checks if categories array has at least one element
  },
  {
    $push: {
      categories: {
        name: "Colleagues",
        description: "",
        isActive: true,
        _id: ObjectId()  // Generates a new ObjectId for the new category
      }
    }
  }
);
```
### Explanation:
__Filter Criteria:__

- _id: ObjectId("67262c4e9be22fd3ef8d2596"): Identifies the document to update.
- "categories.0": { $exists: true }: Ensures that the categories array has at least one element before performing the update.

- Update Operation:
  - $push: Adds a new object to the categories array.
  - The new object contains a name, description, isActive status, and a unique _id generated by ObjectId().
    
This approach keeps the existing elements in categories and appends the new element to the array.

### To delete an element from an array in MongoDB
To delete an element from an array in MongoDB, you can use the $pull operator, which removes all elements that match a specified condition. Here’s how to delete a specific element from the categories array in the expenseType collection:

Assume you want to delete the category with name: "Friends".

``` javascript
db.expenseType.updateOne(
  { _id: ObjectId("67262c4e9be22fd3ef8d2596") },  // specify the document by _id or other criteria
  { $pull: { categories: { name: "Friends" } } }    // specify the condition to match the element to remove
)
```

__Explanation:__
- $pull removes elements from an array that match the specified condition.
- In this case, it will remove any object from categories where name is "Friends".
- Important Note:
  -If there could be multiple entries with "Friends", the $pull operation would remove all matching entries. If you want to       delete only a specific entry (e.g., by _id), you could modify the condition accordingly:

``` javascript
db.expenseType.updateOne(
  { _id: ObjectId("67262c4e9be22fd3ef8d2596") },
  { $pull: { categories: { _id: ObjectId("67262c4e9be22fd3ef8d2598") } } }
)
```
This will remove only the category with the specified _id.
