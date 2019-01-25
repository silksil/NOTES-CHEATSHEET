Let's say we delete an item. If we run a mutation it will be deleted in the back-end, but the cache doesn't automatically update. If you want to update the cache you can 1.) refetch the entire query or 2.) you want to remove 1 or 2 items from the page.

### Refetch entire query

### Remove 1 or 2 items from page
The Mutation property has a update function. Apollo will give you 2 properties if the update happens:
1. The cache
2. The payload

```js
update = (cache, payload) => {
  // manually update the cache on the client, so it matches the server
  // 1. Read the cache for the items we want
  const data = cache.readQuery({ query: ALL_ITEMS_QUERY });
  // 2. Filter the deleted item out of the page
  data.items = data.items.filter(item => item.id !== payload.data.deleteItem.id);
  // 3. Put the items back!
  cache.writeQuery({ query: ALL_ITEMS_QUERY, data });
};
```