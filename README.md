# useQueries tRPC bug

A small bug that causes tRPC to miss the query options that are passed to useQueries. Live example: [#](https://showing-usequeries-bug.vercel.app/).

### Issue:

  ```js
  const hello = api.useQueries((t) => {
    return ["A", "B", "C"].map((x) =>
      t.example.hello(
        { text: x },
        {
          enabled: false,
          onSuccess: () => {
            // I can write whatever I want here, it will never execute!
            console.log("QUERY", x, "COMPLETED");
          },
        }
      )
    );
  });
  ```

Because of this bug, tRPC completely ignores the options object. The queries will be executed "normally", they will not be disabled and the callback will never be called.

### Temporary fix:

This is not the "right" way to do things, but until it's fixed you can edit the query object directly:

  ```js
  const hello = api.useQueries((t) => {
    return ["A", "B", "C"].map((x) => {
      const query = t.example.hello({ text: x });
      query.enabled = false;
      return query;
    });
  });
  ```
