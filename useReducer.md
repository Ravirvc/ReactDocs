### `useReducer` Hook in React

`useReducer` is a React hook that provides a more powerful alternative to `useState` for managing state, especially when the state logic becomes complex, or when different actions need to modify the state in various ways. It's based on the reducer pattern, commonly used in state management libraries like Redux, where a "reducer" function determines how state should change based on the dispatched action.

### Why Use `useReducer`?

- **Complex State Logic**: When your state transitions (like toggling, incrementing, or setting different values) become hard to manage using multiple `useState` hooks, `useReducer` provides a cleaner way to handle it.
  
- **Multiple State Updates**: If multiple actions or events update the same piece of state, `useReducer` helps by allowing you to centralize the logic into a single reducer function.

- **Predictable State Changes**: Since the state transitions are handled by the reducer function, the state changes become predictable and easy to debug.

### Syntax of `useReducer`

```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

- `state`: The current state of your component.
- `dispatch`: A function to dispatch an action (which will trigger the reducer function).
- `reducer`: A function that takes the current state and an action and returns the new state.
- `initialState`: The initial state of the component.

### `reducer` Function

The reducer function takes two arguments:
1. **`state`**: The current state of the application.
2. **`action`**: An object that describes what to do. It typically has a `type` property and an optional `payload`.

Example of a reducer function:

```javascript
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}
```

### Example: Counter using `useReducer`

Here’s an example of a simple counter app using `useReducer`:

```jsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}

export default Counter;
```

### How It Works:
1. **Initial State**: The component starts with an initial state `{ count: 0 }`.
2. **Reducer Function**: The `reducer` function decides how the state should change based on the dispatched action.
3. **Dispatching Actions**: The buttons dispatch actions (`increment` or `decrement`), and the reducer updates the state accordingly.

### Benefits of `useReducer` over `useState`:
- **Scalability**: For large components with complex state logic, `useReducer` makes the logic more manageable by centralizing it in one function.
- **Predictability**: State changes are predictable, as all logic resides within the `reducer` function.
- **Better for Complex State**: If your state depends on multiple sub-values or complicated interactions, `useReducer` can be more appropriate than managing multiple `useState` calls.

### When to Use `useReducer`:
- When the state logic is complex or depends on multiple state variables.
- When you need to manage state transitions based on different actions.
- When state updates are tightly related and easier to handle in one place.

If the state logic is simple (like a boolean toggle), `useState` might be sufficient, but `useReducer` shines in cases with complex interactions and state management needs.

### Comparing `useState` and `useReducer` with Examples

To better illustrate the difference between `useState` and `useReducer`, let's consider a couple of scenarios where using `useReducer` simplifies the state management compared to `useState`. The key benefit is that `useReducer` centralizes state update logic, making it easier to maintain and scale as the complexity grows.

---

### Example 1: Managing Multiple States
Let’s say you want to manage the state of a form with a few fields like `name`, `age`, and `email`.

#### Using `useState`
If you were to use `useState`, you would need a separate state and setter function for each field:

```jsx
import React, { useState } from 'react';

function FormWithUseState() {
  const [name, setName] = useState('');
  const [age, setAge] = useState('');
  const [email, setEmail] = useState('');

  const handleReset = () => {
    setName('');
    setAge('');
    setEmail('');
  };

  return (
    <div>
      <input type="text" placeholder="Name" value={name} onChange={(e) => setName(e.target.value)} />
      <input type="number" placeholder="Age" value={age} onChange={(e) => setAge(e.target.value)} />
      <input type="email" placeholder="Email" value={email} onChange={(e) => setEmail(e.target.value)} />
      <button onClick={handleReset}>Reset</button>
    </div>
  );
}

export default FormWithUseState;
```

- Here, each field has its own state and a corresponding setter function (`setName`, `setAge`, `setEmail`).
- Updating and resetting each field individually can quickly become tedious as more fields are added.

#### Using `useReducer`
Instead, with `useReducer`, you can manage all fields in a single state object. The reducer function centralizes the update logic:

```jsx
import React, { useReducer } from 'react';

const initialState = {
  name: '',
  age: '',
  email: '',
};

function reducer(state, action) {
  switch (action.type) {
    case 'SET_NAME':
      return { ...state, name: action.payload };
    case 'SET_AGE':
      return { ...state, age: action.payload };
    case 'SET_EMAIL':
      return { ...state, email: action.payload };
    case 'RESET':
      return initialState;
    default:
      return state;
  }
}

function FormWithUseReducer() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <input
        type="text"
        placeholder="Name"
        value={state.name}
        onChange={(e) => dispatch({ type: 'SET_NAME', payload: e.target.value })}
      />
      <input
        type="number"
        placeholder="Age"
        value={state.age}
        onChange={(e) => dispatch({ type: 'SET_AGE', payload: e.target.value })}
      />
      <input
        type="email"
        placeholder="Email"
        value={state.email}
        onChange={(e) => dispatch({ type: 'SET_EMAIL', payload: e.target.value })}
      />
      <button onClick={() => dispatch({ type: 'RESET' })}>Reset</button>
    </div>
  );
}

export default FormWithUseReducer;
```

**Benefits of Using `useReducer`:**
- The entire state is managed in one object.
- State update logic is centralized in the `reducer` function.
- You only need one `dispatch` function to handle all changes.

---

### Example 2: Toggling Multiple Boolean States
Suppose you have a UI where you need to show/hide various sections like a sidebar, a modal, and a dropdown.

#### Using `useState`
With `useState`, you would manage each boolean value separately:

```jsx
import React, { useState } from 'react';

function ToggleWithUseState() {
  const [isSidebarOpen, setSidebarOpen] = useState(false);
  const [isModalOpen, setModalOpen] = useState(false);
  const [isDropdownOpen, setDropdownOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setSidebarOpen(!isSidebarOpen)}>Toggle Sidebar</button>
      <button onClick={() => setModalOpen(!isModalOpen)}>Toggle Modal</button>
      <button onClick={() => setDropdownOpen(!isDropdownOpen)}>Toggle Dropdown</button>

      <p>Sidebar is {isSidebarOpen ? 'Open' : 'Closed'}</p>
      <p>Modal is {isModalOpen ? 'Open' : 'Closed'}</p>
      <p>Dropdown is {isDropdownOpen ? 'Open' : 'Closed'}</p>
    </div>
  );
}

export default ToggleWithUseState;
```

**Problems:**
- As more UI elements are added, the number of states and `set` functions can become cumbersome.
- Each toggle action has its own logic scattered throughout the component.

#### Using `useReducer`
With `useReducer`, you can handle all the toggles in a single state object, and the toggle logic is centralized in the reducer:

```jsx
import React, { useReducer } from 'react';

const initialState = {
  isSidebarOpen: false,
  isModalOpen: false,
  isDropdownOpen: false,
};

function reducer(state, action) {
  switch (action.type) {
    case 'TOGGLE_SIDEBAR':
      return { ...state, isSidebarOpen: !state.isSidebarOpen };
    case 'TOGGLE_MODAL':
      return { ...state, isModalOpen: !state.isModalOpen };
    case 'TOGGLE_DROPDOWN':
      return { ...state, isDropdownOpen: !state.isDropdownOpen };
    default:
      return state;
  }
}

function ToggleWithUseReducer() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <button onClick={() => dispatch({ type: 'TOGGLE_SIDEBAR' })}>Toggle Sidebar</button>
      <button onClick={() => dispatch({ type: 'TOGGLE_MODAL' })}>Toggle Modal</button>
      <button onClick={() => dispatch({ type: 'TOGGLE_DROPDOWN' })}>Toggle Dropdown</button>

      <p>Sidebar is {state.isSidebarOpen ? 'Open' : 'Closed'}</p>
      <p>Modal is {state.isModalOpen ? 'Open' : 'Closed'}</p>
      <p>Dropdown is {state.isDropdownOpen ? 'Open' : 'Closed'}</p>
    </div>
  );
}

export default ToggleWithUseReducer;
```

**Benefits of Using `useReducer`:**
- All the toggles are stored in one state object.
- State update logic is consolidated, making it easy to manage and debug.
- Adding a new toggle is as simple as adding a new action type in the reducer.

---

### When to Use `useState` vs `useReducer`

- **Use `useState`** when:
  - The state is simple and involves just a few pieces of data.
  - State updates are straightforward (e.g., toggling a single value or updating one input field).
  - You want quick and easy state management for individual components.

- **Use `useReducer`** when:
  - The state is complex or has multiple sub-values (e.g., a form with multiple inputs).
  - You need to handle multiple state updates based on different actions.
  - The state update logic is shared across components.
  - You want a more structured way to handle actions and state transitions.

### Using `useReducer` with Context in React

When combining `useReducer` with the React Context API, you can create a global state management solution similar to Redux, but using native React hooks. This is useful for managing complex state and sharing it across multiple components without passing props down the component tree.

### Why Use `useReducer` with Context?

- **Centralized State Management**: Manage and access state from anywhere in your component tree.
- **Scalability**: Good for large applications where state needs to be shared across many components.
- **Better State Management Pattern**: Encapsulate state logic in a reducer function and dispatch actions to trigger state changes, making the code easier to maintain and scale.

### Setting Up `useReducer` with Context

To illustrate, let’s build a basic counter application where the counter state is shared across multiple components using `useReducer` and Context.

---

### Step 1: Create the Reducer and Context

1. Create a new file `CounterContext.js` in your `src` folder:

```jsx
import React, { createContext, useReducer, useContext } from 'react';

// Define initial state for the reducer
const initialState = { count: 0 };

// Create a reducer function to handle state changes
const reducer = (state, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    case 'RESET':
      return initialState;
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
};

// Create Context for the state
const CounterContext = createContext();

// Create a custom hook to use the CounterContext
export const useCounter = () => useContext(CounterContext);

// Create a Provider component that wraps the children components
export const CounterProvider = ({ children }) => {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return (
    <CounterContext.Provider value={{ state, dispatch }}>
      {children}
    </CounterContext.Provider>
  );
};
```

- **`CounterContext`**: The context object used to provide and consume state.
- **`CounterProvider`**: A context provider component that initializes the reducer and passes down the `state` and `dispatch`.
- **`useCounter`**: A custom hook to easily access `state` and `dispatch` in child components.

### Step 2: Use the Provider in Your App

Wrap your main application or part of your component tree with the `CounterProvider`:

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { CounterProvider } from './CounterContext';

ReactDOM.render(
  <CounterProvider>
    <App />
  </CounterProvider>,
  document.getElementById('root')
);
```

Now the entire `App` component (and all its children) will have access to the shared counter state.

### Step 3: Create Components that Consume the Context

Let’s create a few components that interact with the shared counter state.

#### `CounterDisplay.js`
This component will display the current count:

```jsx
import React from 'react';
import { useCounter } from './CounterContext';

function CounterDisplay() {
  const { state } = useCounter(); // Use the custom hook to access state

  return <h1>Current Count: {state.count}</h1>;
}

export default CounterDisplay;
```

#### `CounterControls.js`
This component will provide buttons to modify the count:

```jsx
import React from 'react';
import { useCounter } from './CounterContext';

function CounterControls() {
  const { dispatch } = useCounter(); // Use the custom hook to access dispatch

  return (
    <div>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>Decrement</button>
      <button onClick={() => dispatch({ type: 'RESET' })}>Reset</button>
    </div>
  );
}

export default CounterControls;
```

### Step 4: Combine Components in `App.js`

Now, let’s combine the components in the main `App` component:

```jsx
import React from 'react';
import CounterDisplay from './CounterDisplay';
import CounterControls from './CounterControls';

function App() {
  return (
    <div className="App">
      <CounterDisplay />
      <CounterControls />
    </div>
  );
}

export default App;
```

### How the Code Works:

- **`CounterContext` and `CounterProvider`**:
  - `CounterProvider` wraps the main component tree and initializes the `useReducer` hook.
  - `CounterContext` provides the `state` and `dispatch` to all child components.
  
- **State and Dispatch Access**:
  - Using the custom `useCounter` hook, child components can easily access `state` and `dispatch`.
  
- **State Management**:
  - `CounterDisplay` reads the current count.
  - `CounterControls` uses the `dispatch` function to trigger state changes like increment, decrement, or reset.

### Benefits of Using `useReducer` with Context:

1. **Global State Management**: All components within the provider have access to the shared state.
2. **Centralized State Logic**: The reducer function centralizes state update logic, making it easier to debug.
3. **Scalable Pattern**: As the application grows, you can extend the reducer function with new state actions without cluttering the components.

### A More Complex Example: To-Do List with `useReducer` and Context

For a more complex example, let’s build a simple to-do list application where we can add, remove, and toggle the completion status of items.

1. **Create a new file `TodoContext.js`**:

```jsx
import React, { createContext, useReducer, useContext } from 'react';

const initialState = { todos: [] };

const reducer = (state, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return { ...state, todos: [...state.todos, { id: Date.now(), text: action.payload, completed: false }] };
    case 'TOGGLE_TODO':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload ? { ...todo, completed: !todo.completed } : todo
        ),
      };
    case 'REMOVE_TODO':
      return { ...state, todos: state.todos.filter(todo => todo.id !== action.payload) };
    default:
      return state;
  }
};

const TodoContext = createContext();

export const useTodos = () => useContext(TodoContext);

export const TodoProvider = ({ children }) => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return <TodoContext.Provider value={{ state, dispatch }}>{children}</TodoContext.Provider>;
};
```

2. **Create components for managing todos: `TodoList.js`, `TodoInput.js`, and `TodoItem.js`**.
   
- **TodoInput.js**: Component to add new todos.
- **TodoList.js**: Component to list and render each `TodoItem`.
- **TodoItem.js**: Individual todo item component that supports toggling and removal.

3. **Wrap your app with `TodoProvider` and use the components to build a complete to-do list application**.

---
This combination of `useReducer` and Context allows you to build scalable, maintainable state management systems in React without the need for external libraries like Redux.
