Creating a custom React hook allows you to reuse logic between components. Custom hooks are JavaScript functions whose names start with "use" and can call other hooks inside them. Here's a simple guide on how to create a custom React hook:

### Steps to Create a Custom Hook

1. **Identify the Logic**: Determine what reusable logic you'd like to encapsulate. For example, fetching data from an API, handling form inputs, or managing local storage.

2. **Create a Function Starting with "use"**: Custom hooks need to follow React's hook naming convention.

3. **Use Built-in Hooks Inside the Custom Hook**: You can use React’s built-in hooks like `useState`, `useEffect`, etc., inside your custom hook.

4. **Return Values**: You can return any data or functions needed from your custom hook, so the component that calls it can use them.

### Example: Custom Hook for Fetching Data

This example creates a custom hook, `useFetch`, to fetch data from an API and return the data, a loading state, and any errors.

```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('Failed to fetch');
        }
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}

export default useFetch;
```

### How to Use the Custom Hook

You can use this custom hook in any component like this:

```jsx
import React from 'react';
import useFetch from './useFetch';

function DataComponent() {
  const { data, loading, error } = useFetch('https://api.example.com/data');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;

  return (
    <div>
      <h1>Fetched Data</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default DataComponent;
```

### Key Points:
- **Custom hooks can call other hooks** like `useState` and `useEffect`.
- **Hooks always start with "use"**, otherwise React won't treat them as hooks.
- You can return multiple values, including state, functions, and derived values.
