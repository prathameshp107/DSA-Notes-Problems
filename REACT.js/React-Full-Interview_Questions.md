# ⚛️ React Complete Interview Guide — Easy → Medium → Hard

> Covers **all levels**: Fresher → Junior → Mid → Senior  
> React 16.8+ (Hooks era) → React 18 → React 19  
> Total: **80+ Questions** with code examples

---

## 📋 Table of Contents

| Level | Topics | Questions |
|---|---|---|
| 🟢 [Easy](#-easy-questions--fresher--junior) | Basics, JSX, Components, Props, State | Q1–Q25 |
| 🟡 [Medium](#-medium-questions--mid-level) | Hooks, Lifecycle, Router, Performance | Q26–Q55 |
| 🔴 [Hard](#-hard-questions--senior--expert) | React 18/19, Architecture, Internals | Q56–Q80 |

---

# 🟢 Easy Questions — Fresher / Junior

> If you're just starting out, master these first.  
> These are asked in **every** React interview regardless of level.

---

## Basics & Core Concepts

---

### Q1. What is React? Why do we use it?

React is an **open-source JavaScript library** made by Meta (Facebook) for building **user interfaces**.

It is NOT a full framework — it only handles the **View layer** (what you see on screen).

**Why React?**

| Problem Without React | How React Solves It |
|---|---|
| Manually updating DOM is slow and error-prone | Virtual DOM handles updates efficiently |
| Code gets messy in large apps | Component-based — break UI into small reusable pieces |
| Hard to share UI logic | Props + custom hooks allow reuse |
| Hard to manage app state | State management built-in |

```
// Without React — manual DOM manipulation
document.getElementById('count').innerText = count;
document.getElementById('btn').addEventListener('click', () => { ... });

// With React — declarative (describe WHAT, not HOW)
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

---

### Q2. What is the difference between a Library and a Framework?

| | Library (React) | Framework (Angular) |
|---|---|---|
| Control | **You** call the library | Framework calls **your** code |
| Flexibility | Pick any tools you want | Follows framework conventions |
| Size | Small — just the view layer | Full package — routing, HTTP, etc. |
| Learning curve | Easier to start | Steeper but more structured |

React = Library. Next.js = Framework built on top of React.

---

### Q3. What is JSX?

JSX = **JavaScript XML** — a syntax extension that lets you write HTML-like code inside JavaScript.

```jsx
// JSX
const element = <h1 className="title">Hello World</h1>;

// What JSX compiles to (under the hood)
const element = React.createElement('h1', { className: 'title' }, 'Hello World');
```

**JSX Rules:**
```jsx
// 1. Must return ONE root element — use Fragment if needed
return (
  <>                      {/* Fragment — renders nothing in DOM */}
    <h1>Title</h1>
    <p>Paragraph</p>
  </>
);

// 2. Use className, not class
<div className="container">

// 3. All tags must be closed
<img src="pic.jpg" />    // self-closing ✅
<br />                   // must close ✅

// 4. JavaScript expressions go in { }
<h1>{user.name}</h1>
<p>{2 + 2}</p>
<button onClick={handleClick}>Click</button>

// 5. camelCase for HTML attributes
<input onChange={fn} maxLength={10} />
```

---

### Q4. What is a Component in React?

A component is an **independent, reusable piece of UI**.  
Think of it like a LEGO brick — you build complex UIs by combining small components.

```jsx
// Functional Component (modern — always use this)
function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Using it
<Welcome name="Prathamesh" />
// Renders: Hello, Prathamesh!
```

**Types of Components:**

```jsx
// 1. Functional Component (use this always)
function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>;
}

// 2. Class Component (old — avoid in new code)
class Button extends React.Component {
  render() {
    return <button onClick={this.props.onClick}>{this.props.label}</button>;
  }
}
```

---

### Q5. What are Props?

Props = **Properties** — data passed FROM parent TO child component.  
Props are **read-only** — a child can never change its own props.

```jsx
// Parent passes data as props
function App() {
  return (
    <UserCard
      name="Prathamesh"
      age={22}
      isAdmin={true}
      onClick={() => alert('clicked')}
    />
  );
}

// Child receives props
function UserCard({ name, age, isAdmin, onClick }) {
  return (
    <div onClick={onClick}>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      {isAdmin && <span>Admin</span>}
    </div>
  );
}
```

**Default Props:**
```jsx
function Button({ label = 'Click me', color = 'blue' }) {
  return <button style={{ color }}>{label}</button>;
}
```

---

### Q6. What is State?

State = **component's own data** that can change over time.  
When state changes → React re-renders the component.

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // [value, setter]

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

---

### Q7. What is the difference between Props and State?

| | Props | State |
|---|---|---|
| Who owns it | Parent component | The component itself |
| Can it change | ❌ No (read-only) | ✅ Yes (via setter) |
| Who can change it | Only the parent | Only the component |
| Passed via | Function arguments | `useState` hook |
| Causes re-render | Yes (when parent re-renders) | Yes (when set) |

```jsx
function Parent() {
  const [theme, setTheme] = useState('dark'); // STATE — Parent owns this
  return <Child theme={theme} />;              // PROP — Child receives it
}

function Child({ theme }) {
  // theme is a prop — Child CANNOT change it
  // theme is state in Parent — Parent CAN change it
  return <div className={theme}>Hello</div>;
}
```

---

### Q8. What is the Virtual DOM?

The Virtual DOM is a **lightweight JavaScript copy** of the real DOM kept in memory.

```
User action (click, type)
        ↓
State changes → React creates NEW Virtual DOM
        ↓
React DIFFS new vs old Virtual DOM (reconciliation)
        ↓
Only the CHANGED parts update in the real DOM
        ↓
Browser paints the minimal change
```

**Why is this fast?**
- Real DOM operations are slow (cause repaints/reflows)
- Comparing JS objects is super fast
- React batches multiple changes into one DOM update

---

### Q9. What is the difference between `React.createElement` and JSX?

They produce the exact same result. JSX is just **syntactic sugar**.

```jsx
// JSX (what you write)
const element = (
  <div className="card">
    <h1>Hello</h1>
  </div>
);

// What Babel compiles it to
const element = React.createElement(
  'div',
  { className: 'card' },
  React.createElement('h1', null, 'Hello')
);

// Both produce the same React element object:
// { type: 'div', props: { className: 'card', children: [...] } }
```

---

### Q10. How do you render a list in React?

Use `.map()` to convert an array into JSX. Always add a unique `key` prop.

```jsx
const fruits = ['Apple', 'Banana', 'Mango', 'Orange'];

function FruitList() {
  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>  // index OK for static lists
      ))}
    </ul>
  );
}

// Better — use unique ID from data
const users = [
  { id: 1, name: 'Prathamesh' },
  { id: 2, name: 'Rahul' },
];

function UserList() {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>  // stable ID ✅
      ))}
    </ul>
  );
}
```

---

### Q11. What is the `key` prop and why is it needed?

Keys help React identify **which list items changed** between renders.

```jsx
// ❌ No key — React can't track items
{todos.map(todo => <TodoItem todo={todo} />)}

// ❌ Index as key — causes bugs when list changes order
{todos.map((todo, i) => <TodoItem key={i} todo={todo} />)}

// ✅ Stable unique ID
{todos.map(todo => <TodoItem key={todo.id} todo={todo} />)}
```

**Why index is bad as key:**
```
Before delete:  [Apple(0), Banana(1), Mango(2)]
Delete Apple:   [Banana(0), Mango(1)]

React sees: key=0 changed content (Apple→Banana)
            key=1 changed content (Banana→Mango)
            key=2 was removed

So React UPDATES 2 items instead of removing 1.
With ID as key, React correctly removes Apple's node.
```

---

### Q12. What is Conditional Rendering?

Show or hide components based on a condition.

```jsx
function UserGreeting({ isLoggedIn, name }) {
  // Method 1: if/else (outside JSX)
  if (!isLoggedIn) {
    return <p>Please log in</p>;
  }

  return (
    <div>
      {/* Method 2: Ternary operator */}
      <h1>{isLoggedIn ? `Welcome, ${name}!` : 'Stranger'}</h1>

      {/* Method 3: && (short-circuit) — show only if true */}
      {isLoggedIn && <button>Logout</button>}

      {/* Method 4: || (fallback) */}
      <p>{name || 'Anonymous'}</p>
    </div>
  );
}
```

---

### Q13. How do you handle events in React?

```jsx
function Form() {
  // Event handlers are camelCase in React
  const handleClick = (e) => {
    e.preventDefault();      // prevent default browser behavior
    e.stopPropagation();     // stop event from bubbling up
    console.log('Clicked!');
  };

  const handleChange = (e) => {
    console.log(e.target.value); // get input value
  };

  const handleSubmit = (e) => {
    e.preventDefault(); // IMPORTANT: prevent page reload on form submit
    console.log('Form submitted');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={handleChange} />
      <button onClick={handleClick}>Click</button>
    </form>
  );
}
```

**Passing arguments to event handlers:**
```jsx
// ❌ Wrong — calls immediately on render
<button onClick={handleDelete(id)}>Delete</button>

// ✅ Correct — wrap in arrow function
<button onClick={() => handleDelete(id)}>Delete</button>
```

---

### Q14. What is a Fragment in React?

A Fragment lets you return multiple elements **without adding an extra DOM node**.

```jsx
// ❌ Extra div in DOM (affects layout/CSS)
return (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);

// ✅ Fragment — no extra DOM node
return (
  <>
    <h1>Title</h1>
    <p>Content</p>
  </>
);

// Long syntax — use when you need a key (in lists)
return (
  <React.Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.description}</dd>
  </React.Fragment>
);
```

---

### Q15. What is `useState`?

`useState` is a hook that lets functional components have **local state**.

```jsx
import { useState } from 'react';

function LoginForm() {
  const [email, setEmail]       = useState('');      // string
  const [password, setPassword] = useState('');      // string
  const [loading, setLoading]   = useState(false);   // boolean
  const [errors, setErrors]     = useState({});      // object
  const [items, setItems]       = useState([]);      // array

  // Updating state
  setEmail('test@example.com');    // replaces the value

  // Updating object state — must spread old state!
  setErrors({ ...errors, email: 'Required' });

  // Functional update — use when new value depends on old
  setLoading(prev => !prev);
}
```

---

### Q16. What is `useEffect`?

`useEffect` handles **side effects** — things that happen outside of rendering  
(API calls, timers, subscriptions, DOM manipulation).

```jsx
import { useEffect, useState } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  // Runs AFTER every render (no dependency array)
  useEffect(() => {
    document.title = 'Profile Page';
  });

  // Runs ONCE after mount (empty array)
  useEffect(() => {
    console.log('Component mounted');
  }, []);

  // Runs when userId changes
  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(r => r.json())
      .then(data => setUser(data));
  }, [userId]);  // re-runs when userId changes

  // Cleanup function (runs on unmount or before next effect)
  useEffect(() => {
    const timer = setInterval(() => console.log('tick'), 1000);
    return () => clearInterval(timer);  // cleanup!
  }, []);

  return <div>{user?.name}</div>;
}
```

---

### Q17. What is `useRef`?

`useRef` returns a mutable object `.current` that persists across renders  
**without causing a re-render** when changed.

```jsx
import { useRef } from 'react';

function TextInput() {
  const inputRef = useRef(null);

  // Use 1: Access DOM element directly
  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus</button>
    </>
  );
}

function Timer() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef(null);

  // Use 2: Store value that doesn't trigger re-render
  const start = () => {
    intervalRef.current = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
  };

  const stop = () => {
    clearInterval(intervalRef.current);
  };

  return (
    <>
      <p>{count}</p>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </>
  );
}
```

---

### Q18. What is `useContext`?

`useContext` lets any component **read data from a Context** without prop drilling.

```jsx
import { createContext, useContext, useState } from 'react';

// 1. Create context
const ThemeContext = createContext('light');

// 2. Provide it at the top level
function App() {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Header />
      <Main />
    </ThemeContext.Provider>
  );
}

// 3. Consume it ANYWHERE in the tree
function Header() {
  const { theme, setTheme } = useContext(ThemeContext);
  return (
    <header className={theme}>
      <button onClick={() => setTheme(t => t === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </header>
  );
}
```

---

### Q19. What is Prop Drilling and why is it a problem?

Prop drilling = passing props **through multiple intermediate components** that don't need them — just to get data to a deeply nested child.

```jsx
// ❌ Prop Drilling — Layout and Sidebar don't need user
// but must pass it down
function App() {
  const user = { name: 'Prathamesh' };
  return <Layout user={user} />;
}

function Layout({ user }) {           // doesn't use user
  return <Sidebar user={user} />;
}

function Sidebar({ user }) {          // doesn't use user
  return <UserAvatar user={user} />;
}

function UserAvatar({ user }) {       // finally uses it
  return <img alt={user.name} />;
}

// ✅ Solution: Context API — UserAvatar reads directly
function UserAvatar() {
  const { user } = useContext(UserContext);
  return <img alt={user.name} />;
}
```

---

### Q20. What is the difference between `null`, `undefined`, and `false` in JSX?

```jsx
function Component() {
  return (
    <div>
      {null}       {/* renders nothing */}
      {undefined}  {/* renders nothing */}
      {false}      {/* renders nothing */}
      {0}          {/* ⚠️ renders "0" — common bug! */}
      {''}         {/* renders empty string */}
      {true}       {/* renders nothing */}
    </div>
  );
}

// Common bug with &&
const count = 0;
{count && <Badge />}   // ❌ renders "0" instead of nothing!
{count > 0 && <Badge />}  // ✅ renders nothing correctly
```

---

### Q21. How do you pass data from Child to Parent?

React only flows data **down** (parent → child via props).  
To send data UP, pass a **callback function** as a prop.

```jsx
function Parent() {
  const [message, setMessage] = useState('');

  // Pass setter as callback prop
  return (
    <div>
      <p>Message from child: {message}</p>
      <Child onSend={setMessage} />
    </div>
  );
}

function Child({ onSend }) {
  const [input, setInput] = useState('');

  const handleClick = () => {
    onSend(input);  // call parent's function with data
  };

  return (
    <>
      <input value={input} onChange={e => setInput(e.target.value)} />
      <button onClick={handleClick}>Send to Parent</button>
    </>
  );
}
```

---

### Q22. What are Controlled and Uncontrolled Components?

```jsx
// Controlled — React state drives the input value
function ControlledInput() {
  const [value, setValue] = useState('');

  return (
    <input
      value={value}                         // React controls this
      onChange={e => setValue(e.target.value)}
    />
  );
}

// Uncontrolled — DOM manages its own state
function UncontrolledInput() {
  const inputRef = useRef(null);

  const handleSubmit = () => {
    alert(inputRef.current.value);  // read when needed
  };

  return (
    <>
      <input ref={inputRef} defaultValue="initial" />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
}
```

Use **controlled** for: validation, formatting, dependent fields.  
Use **uncontrolled** for: file inputs, simple forms without validation.

---

### Q23. What is `React.StrictMode`?

A wrapper that activates **extra development-only checks** to find potential problems.

```jsx
// In main.jsx / index.jsx
ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

**What it does in development:**
- Calls component functions **twice** to detect side effects
- Warns about deprecated lifecycle methods
- Warns about legacy Context API usage
- Warns about string refs

> **Note:** It does NOTHING in production. Double-invocation is dev-only.

---

### Q24. What is the difference between `export default` and `named export`?

```jsx
// Named export — must import with exact name
export function Button() { ... }
export const API_URL = 'https://api.example.com';

// Import named export
import { Button, API_URL } from './components';

// Default export — import with any name
export default function App() { ... }

// Import default export
import App from './App';
import MyApp from './App';  // any name works
import WhateverName from './App';  // same component

// A file can have ONE default + MANY named exports
export default function Page() { ... }
export const meta = { title: 'Home' };
```

---

### Q25. What is `children` prop?

`children` is a special prop — it's whatever is **placed between opening and closing tags** of your component.

```jsx
// Card component that wraps any content
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-body">
        {children}    {/* renders whatever is inside <Card>...</Card> */}
      </div>
    </div>
  );
}

// Usage
function App() {
  return (
    <Card title="User Profile">
      <img src="avatar.jpg" />
      <p>Name: Prathamesh</p>
      <button>Edit</button>
      {/* All of the above becomes children */}
    </Card>
  );
}
```

---

# 🟡 Medium Questions — Mid Level

> Assumes you know the basics.  
> These are asked for **1-3 years experience** roles.

---

## Hooks Deep Dive

---

### Q26. Explain all `useEffect` dependency array cases

```jsx
// Case 1: No array — runs after EVERY render
useEffect(() => {
  console.log('renders every time');
});
// Use: rarely needed, almost always a bug

// Case 2: Empty array — runs ONCE after mount
useEffect(() => {
  fetchInitialData();
  setupEventListeners();
  return () => removeEventListeners();
}, []);
// Use: initial data fetch, one-time setup

// Case 3: With dependencies — runs when deps change
useEffect(() => {
  fetchUser(userId);
}, [userId]);
// Use: re-fetch when ID changes, react to prop changes

// Case 4: Object/array as dependency — careful!
// New object = new reference = effect runs every time!
const options = { page: 1 }; // new object every render

useEffect(() => {
  fetchData(options);
}, [options]); // ❌ runs every render!

// Fix: primitive values or useMemo
useEffect(() => {
  fetchData({ page });
}, [page]); // ✅ primitive — only runs when page changes
```

---

### Q27. What is `useMemo`?

`useMemo` **memoizes (caches) an expensive computed value**.  
Only recalculates when its dependencies change.

```jsx
import { useMemo, useState } from 'react';

function ProductList({ products, searchQuery }) {
  // Without useMemo — filters on EVERY render (even unrelated re-renders)
  const filtered = products.filter(p =>
    p.name.toLowerCase().includes(searchQuery.toLowerCase())
  );

  // With useMemo — only recalculates when products or searchQuery changes
  const filteredProducts = useMemo(() => {
    console.log('Filtering...'); // only logs when deps change
    return products.filter(p =>
      p.name.toLowerCase().includes(searchQuery.toLowerCase())
    );
  }, [products, searchQuery]);

  return (
    <ul>
      {filteredProducts.map(p => <li key={p.id}>{p.name}</li>)}
    </ul>
  );
}
```

**When to use:** Expensive calculations, sorted/filtered lists, derived data.  
**When NOT to use:** Simple calculations — the memo overhead costs more than the calculation.

---

### Q28. What is `useCallback`?

`useCallback` **memoizes a function reference**.  
Returns the same function instance between renders.

```jsx
import { useCallback, useState, memo } from 'react';

// Child wrapped in memo — only re-renders when props change
const TodoItem = memo(function({ todo, onDelete }) {
  console.log('TodoItem rendered', todo.id);
  return (
    <li>
      {todo.text}
      <button onClick={() => onDelete(todo.id)}>Delete</button>
    </li>
  );
});

function TodoList() {
  const [todos, setTodos] = useState([...]);
  const [filter, setFilter] = useState('all');

  // ❌ Without useCallback — new function every render
  // → memo on TodoItem is useless (new prop = always re-renders)
  const handleDelete = (id) => {
    setTodos(prev => prev.filter(t => t.id !== id));
  };

  // ✅ With useCallback — same function reference between renders
  // → memo on TodoItem actually works
  const handleDelete = useCallback((id) => {
    setTodos(prev => prev.filter(t => t.id !== id));
  }, []); // no deps — setTodos is stable

  return todos.map(todo => (
    <TodoItem key={todo.id} todo={todo} onDelete={handleDelete} />
  ));
}
```

---

### Q29. What is `useReducer`?

`useReducer` manages **complex state logic** using a reducer function.  
Like Redux but local to one component.

```jsx
import { useReducer } from 'react';

// Reducer — pure function: (state, action) => newState
function cartReducer(state, action) {
  switch (action.type) {
    case 'ADD_ITEM':
      return {
        ...state,
        items: [...state.items, action.payload],
        total: state.total + action.payload.price,
      };
    case 'REMOVE_ITEM':
      const item = state.items.find(i => i.id === action.payload);
      return {
        ...state,
        items: state.items.filter(i => i.id !== action.payload),
        total: state.total - (item?.price || 0),
      };
    case 'CLEAR_CART':
      return { items: [], total: 0 };
    default:
      return state;
  }
}

function Cart() {
  const [state, dispatch] = useReducer(cartReducer, { items: [], total: 0 });

  return (
    <div>
      <p>Total: ₹{state.total}</p>
      <button onClick={() => dispatch({ type: 'ADD_ITEM', payload: { id: 1, name: 'Shirt', price: 499 } })}>
        Add Shirt
      </button>
      <button onClick={() => dispatch({ type: 'CLEAR_CART' })}>
        Clear
      </button>
    </div>
  );
}
```

---

### Q30. What are Custom Hooks?

A custom hook is a **function starting with `use`** that extracts and reuses stateful logic.

```jsx
// Custom hook — reusable fetch logic
function useFetch(url) {
  const [data, setData]       = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError]     = useState(null);

  useEffect(() => {
    let isMounted = true;
    setLoading(true);

    fetch(url)
      .then(r => {
        if (!r.ok) throw new Error(`HTTP ${r.status}`);
        return r.json();
      })
      .then(d => { if (isMounted) setData(d); })
      .catch(e => { if (isMounted) setError(e.message); })
      .finally(() => { if (isMounted) setLoading(false); });

    return () => { isMounted = false; };
  }, [url]);

  return { data, loading, error };
}

// Usage — clean and reusable in ANY component
function UserProfile({ id }) {
  const { data: user, loading, error } = useFetch(`/api/users/${id}`);

  if (loading) return <p>Loading...</p>;
  if (error)   return <p>Error: {error}</p>;
  return <h1>{user.name}</h1>;
}

function PostList({ userId }) {
  const { data: posts, loading } = useFetch(`/api/posts?user=${userId}`);

  if (loading) return <p>Loading posts...</p>;
  return posts.map(post => <PostCard key={post.id} post={post} />);
}
```

---

### Q31. Rules of Hooks — what are they and why?

**Two rules:**

1. Only call hooks **at the top level** (not inside if/loops/nested functions)
2. Only call hooks **from React function components or custom hooks**

```jsx
// ❌ WRONG — hook inside condition
function Component({ isLoggedIn }) {
  if (isLoggedIn) {
    const [data, setData] = useState(null); // BREAKS rules
  }
}

// ❌ WRONG — hook inside loop
function Component({ count }) {
  for (let i = 0; i < count; i++) {
    const [val] = useState(i); // BREAKS rules
  }
}

// ❌ WRONG — hook in regular function
function regularFunction() {
  const [state] = useState(0); // NOT a component or custom hook
}

// ✅ CORRECT
function Component({ isLoggedIn }) {
  const [data, setData] = useState(null); // always at top level
  // Use the condition inside, not around the hook
  useEffect(() => {
    if (isLoggedIn) fetchData();
  }, [isLoggedIn]);
}
```

**Why this rule exists:**  
React tracks hooks by their **call order** in an internal array. Every render must call hooks in the same order. Conditions/loops break that order → React loses track of which state belongs to which hook.

---

### Q32. What is `React.memo`?

`React.memo` is a Higher Order Component that **skips re-render** if props haven't changed.

```jsx
import { memo } from 'react';

// Without memo — re-renders whenever parent renders
function UserCard({ name, email }) {
  console.log('UserCard rendered');
  return <div>{name} — {email}</div>;
}

// With memo — skips re-render if name and email are same
const UserCard = memo(function({ name, email }) {
  console.log('UserCard rendered');
  return <div>{name} — {email}</div>;
});

// Custom comparison function (optional)
const UserCard = memo(
  function({ user }) { return <div>{user.name}</div>; },
  (prevProps, nextProps) => prevProps.user.id === nextProps.user.id
  // return true = skip re-render, return false = re-render
);
```

---

### Q33. What is the difference between `useEffect` and `useLayoutEffect`?

```jsx
// useEffect — runs AFTER browser paints
// Use for: data fetching, subscriptions, most side effects
useEffect(() => {
  fetchData();
}, []);

// useLayoutEffect — runs AFTER DOM update but BEFORE browser paints
// Use for: DOM measurements, animations that need layout info
useLayoutEffect(() => {
  const height = elementRef.current.offsetHeight;
  // Element dimensions are stable here — browser hasn't painted yet
  setHeight(height);
}, []);
```

**Timeline:**
```
State change
    → React updates DOM
    → useLayoutEffect runs (synchronous) ← DOM measurement here
    → Browser paints (user sees update)
    → useEffect runs (async)
```

**Rule:** Use `useLayoutEffect` only when you see a visual flicker with `useEffect`.

---

## Component Patterns

---

### Q34. What is Lifting State Up?

When two sibling components need to share state, **move the state to their common parent**.

```jsx
// ❌ Each component has its own temp — they don't sync
function Celsius() {
  const [temp, setTemp] = useState(0);
  return <input value={temp} onChange={e => setTemp(e.target.value)} />;
}

function Fahrenheit() {
  const [temp, setTemp] = useState(32);
  return <input value={temp} />;
}

// ✅ Lift state to parent — single source of truth
function TemperatureConverter() {
  const [celsius, setCelsius] = useState(0);

  const fahrenheit = celsius * 9/5 + 32;

  return (
    <>
      <CelsiusInput value={celsius} onChange={setCelsius} />
      <FahrenheitInput value={fahrenheit} />
    </>
  );
}
```

---

### Q35. What are Error Boundaries?

Components that catch JS errors in their **child tree** and show a fallback UI instead of crashing.

```jsx
// Error Boundary — must be a class component
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  // Called when a child throws
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  // Called with error info — use for logging
  componentDidCatch(error, info) {
    console.error('Error:', error);
    logToService(error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

// Usage
<ErrorBoundary>
  <UserProfile />       {/* if this throws, error boundary catches it */}
  <PostList />
</ErrorBoundary>
```

**What Error Boundaries do NOT catch:**
- Event handler errors (use try/catch)
- Async errors (setTimeout, promises)
- Server-side rendering errors

---

### Q36. What is `React.lazy` and Code Splitting?

Load components **only when needed** — reduces initial bundle size.

```jsx
import { lazy, Suspense } from 'react';

// Instead of static import (loads everything upfront)
// import Dashboard from './Dashboard';

// Lazy import — Dashboard.js only downloads when user visits /dashboard
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Settings  = lazy(() => import('./pages/Settings'));
const Profile   = lazy(() => import('./pages/Profile'));

function App() {
  return (
    <Suspense fallback={<div>Loading page...</div>}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings"  element={<Settings />} />
        <Route path="/profile"   element={<Profile />} />
      </Routes>
    </Suspense>
  );
}
// Initial bundle: ~50KB instead of ~200KB
// Each page loads only when visited
```

---

### Q37. What is Context API and when should you use it vs Redux?

```
When to use Context:
✅ Theme (light/dark)
✅ Current user / auth state
✅ Language / locale
✅ Data shared by many components at different levels

When to use Redux/Zustand instead:
✅ Complex state logic with many transitions
✅ State updated from many places
✅ Need time-travel debugging
✅ Large teams where predictability matters

Context performance note:
⚠️ Every consumer re-renders when context value changes
⚠️ Split contexts by update frequency to avoid this
```

```jsx
// Split contexts — theme rarely changes, user might
const ThemeContext  = createContext(); // slow-changing
const UserContext   = createContext(); // fast-changing

// Components only re-render for the context they use
```

---

### Q38. What is `forwardRef`?

Lets you pass a `ref` through a component to a child DOM element.

```jsx
import { forwardRef, useRef } from 'react';

// Without forwardRef, ref won't reach the actual input
const CustomInput = forwardRef(function(props, ref) {
  return (
    <div className="input-wrapper">
      <input ref={ref} {...props} className="fancy-input" />
    </div>
  );
});

// Now parent can access the actual input DOM node
function Form() {
  const inputRef = useRef(null);

  const focusInput = () => inputRef.current.focus();

  return (
    <>
      <CustomInput ref={inputRef} placeholder="Type here..." />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
}
```

---

### Q39. What is the difference between `useState` setter calling twice in StrictMode?

In **React 18 StrictMode**, React intentionally calls your component function **twice** in development.

```jsx
function Counter() {
  const [count, setCount] = useState(() => {
    console.log('Initializer runs twice in StrictMode dev'); // expected!
    return 0;
  });

  useEffect(() => {
    console.log('Effect runs twice in StrictMode dev'); // expected!
    // Mount → run → unmount → mount → run
    // This helps detect missing cleanup functions
  }, []);
}
```

**Why:** To detect side effects in render and help you write pure components.  
**In production:** Runs only once. This is only development behavior.

---

## React Router v6

---

### Q40. What is the difference between `<Link>` and `<NavLink>`?

```jsx
import { Link, NavLink } from 'react-router-dom';

// Link — basic navigation, no active state
<Link to="/about">About</Link>

// NavLink — adds active class/style when route matches
<NavLink
  to="/about"
  className={({ isActive }) => isActive ? 'nav-active' : 'nav-link'}
>
  About
</NavLink>

// NavLink with inline style
<NavLink
  to="/dashboard"
  style={({ isActive }) => ({
    color: isActive ? '#007bff' : '#333',
    fontWeight: isActive ? 'bold' : 'normal',
  })}
>
  Dashboard
</NavLink>
```

---

### Q41. What is `<Outlet>` in React Router v6?

`Outlet` is a **placeholder** in a parent layout where child routes render.

```jsx
// routes
<Route path="/" element={<Layout />}>
  <Route path="home"      element={<Home />} />
  <Route path="dashboard" element={<Dashboard />} />
  <Route path="settings"  element={<Settings />} />
</Route>

// Layout.jsx — the frame that never changes
function Layout() {
  return (
    <div>
      <nav>
        <NavLink to="/home">Home</NavLink>
        <NavLink to="/dashboard">Dashboard</NavLink>
        <NavLink to="/settings">Settings</NavLink>
      </nav>
      <main>
        <Outlet />  {/* Home / Dashboard / Settings renders here */}
      </main>
      <footer>© 2025</footer>
    </div>
  );
}
```

---

### Q42. How do you do programmatic navigation?

```jsx
import { useNavigate } from 'react-router-dom';

function LoginPage() {
  const navigate = useNavigate();

  const handleLogin = async (e) => {
    e.preventDefault();
    await login(email, password);

    navigate('/dashboard');               // go to dashboard
    navigate(-1);                         // go back
    navigate(1);                          // go forward
    navigate('/login', { replace: true }); // replace current history entry
    navigate('/profile', {
      state: { from: 'login', message: 'Welcome back!' }
    });                                   // pass state
  };

  // Read state in destination
  const location = useLocation();
  console.log(location.state?.message);  // 'Welcome back!'
}
```

---

## State Management

---

### Q43. What is Redux? Explain its core concepts.

```
Redux Core Concepts:

1. Store   — single object holding ALL app state
2. Action  — plain object describing what happened { type, payload }
3. Reducer — pure function: (state, action) => newState
4. Dispatch — the only way to trigger state change
5. Selector — function to read specific data from store
```

```jsx
// Modern Redux Toolkit (use this, not old Redux)

// 1. Create slice (state + reducers + actions together)
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => { state.value += 1; },
    decrement: state => { state.value -= 1; },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
});

// 2. Create store
const store = configureStore({
  reducer: { counter: counterSlice.reducer }
});

// 3. In component
function Counter() {
  const count    = useSelector(state => state.counter.value);
  const dispatch = useDispatch();

  return (
    <>
      <p>{count}</p>
      <button onClick={() => dispatch(counterSlice.actions.increment())}>+</button>
      <button onClick={() => dispatch(counterSlice.actions.incrementByAmount(5))}>+5</button>
    </>
  );
}
```

---

### Q44. What is Zustand and how is it different from Redux?

```jsx
// Zustand — minimal state management
import { create } from 'zustand';

const useStore = create((set) => ({
  // State
  count: 0,
  user: null,

  // Actions live inside the store
  increment: () => set(state => ({ count: state.count + 1 })),
  setUser:   (user) => set({ user }),
  reset:     () => set({ count: 0, user: null }),
}));

// No Provider needed — just import and use
function Counter() {
  const count     = useStore(state => state.count);
  const increment = useStore(state => state.increment);

  return <button onClick={increment}>{count}</button>;
}
```

---

## Performance

---

### Q45. What causes unnecessary re-renders and how to fix them?

```jsx
// Cause 1: Object/array created in render
function Parent() {
  // ❌ New object every render → child re-renders
  return <Child options={{ theme: 'dark' }} />;

  // ✅ Memoize the object
  const options = useMemo(() => ({ theme: 'dark' }), []);
  return <Child options={options} />;
}

// Cause 2: Inline function in render
function Parent() {
  // ❌ New function every render → child re-renders
  return <Child onClick={() => handleClick(id)} />;

  // ✅ useCallback
  const handleClick = useCallback(() => doThing(id), [id]);
  return <Child onClick={handleClick} />;
}

// Cause 3: Context value changing too often
// ❌ New object every render → ALL consumers re-render
<MyContext.Provider value={{ user, setUser }}>

// ✅ Memoize context value
const value = useMemo(() => ({ user, setUser }), [user]);
<MyContext.Provider value={value}>
```

---

### Q46. What is Reconciliation?

The algorithm React uses to **efficiently update the DOM** by comparing old and new virtual DOM trees.

**Key rules:**
1. Elements of **different types** → destroy old, build new
2. Elements of **same type** → update only changed attributes
3. For **lists** → use `key` prop to match items

```jsx
// Rule 1 — type change: destroys entire subtree
// Before: <div><Counter /></div>
// After:  <span><Counter /></span>
// → Counter is destroyed and remounted (state reset!)

// Rule 2 — same type: only updates changed attributes
// Before: <div className="red" id="box" />
// After:  <div className="blue" id="box" />
// → React only updates className, not id

// Rule 3 — keys help match list items
// Before: [Apple(id:1), Banana(id:2)]
// After:  [Banana(id:2), Apple(id:1)]  ← order changed
// With key: React moves DOM nodes, no re-render of content ✅
// Without key: React thinks both items changed → re-renders both ❌
```

---

### Q47. What is the `key` prop and what happens when it changes?

When a component's `key` prop changes, React **completely destroys and recreates** it — this resets all internal state.

```jsx
// Intentional use: reset a component by changing its key
function ParentForm() {
  const [userId, setUserId] = useState(1);

  return (
    <>
      <button onClick={() => setUserId(2)}>Switch User</button>
      {/* key change forces UserForm to reset — clears all fields */}
      <UserForm key={userId} userId={userId} />
    </>
  );
}
```

---

# 🔴 Hard Questions — Senior / Expert

> These are asked for **3+ years experience** or senior roles.  
> Covers React internals, concurrent features, and architecture.

---

### Q48. What is the React Fiber architecture?

Fiber is React's **internal reconciliation engine** (rewritten in React 16).

**Before Fiber (React 15):** Reconciliation was synchronous and couldn't be interrupted.  
A huge component tree update would block the main thread → janky UI.

**After Fiber (React 16+):** Reconciliation is **incremental and interruptible**.

```
Fiber = a linked list of units of work (one node per component)

React can:
  - Pause reconciliation
  - Resume it later
  - Abort it entirely
  - Prioritize urgent updates (user input) over background updates (data fetch)

This enables:
  - useTransition (defer non-urgent updates)
  - Suspense (pause rendering while waiting)
  - Concurrent rendering (prepare multiple UI versions in memory)
```

---

### Q49. What is Concurrent Mode / Concurrent Rendering?

React 18 introduced the ability to **prepare multiple versions of the UI simultaneously** without blocking the main thread.

```jsx
// createRoot enables concurrent rendering
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);

// Now React can:
// 1. Interrupt a low-priority render for a high-priority one
// 2. Reuse previous renders if state didn't change
// 3. Discard renders that become stale

// useTransition uses concurrent rendering
const [isPending, startTransition] = useTransition();

startTransition(() => {
  // React prepares this update in background
  // If user types again, React throws away this work and starts fresh
  setSearchQuery(value);
});
```

---

### Q50. What is `useTransition` and `useDeferredValue`? When to use each?

```jsx
// useTransition — wrap the STATE SETTER as non-urgent
function SearchPage() {
  const [input, setInput]   = useState('');
  const [query, setQuery]   = useState('');
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    setInput(e.target.value);  // urgent — update input immediately

    startTransition(() => {
      setQuery(e.target.value); // non-urgent — can be interrupted
    });
  };

  return (
    <>
      <input value={input} onChange={handleChange} />
      {isPending ? <Spinner /> : <ResultsList query={query} />}
    </>
  );
}

// useDeferredValue — wrap the VALUE when you don't control the setter
function ResultsList({ query }) {
  const deferredQuery = useDeferredValue(query);
  // deferredQuery lags behind query
  // React shows stale results while computing new ones

  const results = useMemo(
    () => filterExpensively(allItems, deferredQuery),
    [deferredQuery]
  );

  const isStale = query !== deferredQuery;
  return <div style={{ opacity: isStale ? 0.7 : 1 }}>{results}</div>;
}
```

---

### Q51. What are React Server Components?

```jsx
// Server Component (default in Next.js app dir)
// Runs ONLY on server — zero JS shipped to browser
// Can be async, access DB, use server-only packages
async function ProductList() {
  const products = await db.select().from(productsTable);
  return (
    <ul>
      {products.map(p => (
        <li key={p.id}>
          {p.name} — ₹{p.price}
          <AddToCartButton id={p.id} />  {/* Client Component for interactivity */}
        </li>
      ))}
    </ul>
  );
}

// Client Component — add 'use client' directive
'use client';
function AddToCartButton({ id }) {
  const [added, setAdded] = useState(false);
  return (
    <button onClick={() => { addToCart(id); setAdded(true); }}>
      {added ? 'Added!' : 'Add to Cart'}
    </button>
  );
}
```

**Rules:**
- Server Components CANNOT use `useState`, `useEffect`, or any hooks
- Server Components CAN be async, access DB, read environment variables
- Client Components CANNOT be async at the component level
- Client Components CAN import Server Components as children via props
- Server Components CANNOT import Client Components directly (must pass as props)

---

### Q52. What is `useOptimistic`? (React 19)

Displays an **optimistic state immediately** while an async operation is in progress.  
Automatically reverts if the operation fails.

```jsx
'use client';

function MessageList({ messages, sendMessage }) {
  const [optimisticMessages, addOptimisticMessage] = useOptimistic(
    messages,
    (state, newMessage) => [...state, { ...newMessage, pending: true }]
  );

  async function handleSend(formData) {
    const text = formData.get('text');

    // Show immediately
    addOptimisticMessage({ id: Date.now(), text, sender: 'me' });

    // Actually save — if fails, optimistic message disappears
    await sendMessage(text);
  }

  return (
    <div>
      {optimisticMessages.map(msg => (
        <div key={msg.id} style={{ opacity: msg.pending ? 0.6 : 1 }}>
          {msg.text} {msg.pending && '(Sending...)'}
        </div>
      ))}
      <form action={handleSend}>
        <input name="text" />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

---

### Q53. What is `useActionState`? (React 19)

Manages async form state (pending, result, error) all in one hook.

```jsx
'use client';
import { useActionState } from 'react';

async function submitForm(prevState, formData) {
  const name  = formData.get('name');
  const email = formData.get('email');

  if (!email.includes('@')) {
    return { error: 'Invalid email', success: false };
  }

  await saveToDatabase({ name, email });
  return { error: null, success: true, message: `Welcome, ${name}!` };
}

function ContactForm() {
  const [state, formAction, isPending] = useActionState(submitForm, {
    error: null,
    success: false,
    message: '',
  });

  return (
    <form action={formAction}>
      <input name="name"  placeholder="Name" required />
      <input name="email" placeholder="Email" required />

      {state.error   && <p style={{ color: 'red'   }}>{state.error}</p>}
      {state.success && <p style={{ color: 'green' }}>{state.message}</p>}

      <button type="submit" disabled={isPending}>
        {isPending ? 'Submitting...' : 'Submit'}
      </button>
    </form>
  );
}
```

---

### Q54. What is Automatic Batching in React 18?

```jsx
// React 17 — only batched inside React event handlers
function handleClick() {
  setCount(c => c + 1);  // render 1
  setFlag(f => !f);      // render 2
}

// React 17 — NO batching in async contexts
setTimeout(() => {
  setCount(c => c + 1);  // render 1
  setFlag(f => !f);      // render 2 — total 2 renders!
}, 1000);

// React 18 — automatic batching EVERYWHERE
setTimeout(() => {
  setCount(c => c + 1);
  setFlag(f => !f);      // 1 render total ✅
}, 1000);

fetch('/api').then(() => {
  setData(d);
  setLoading(false);     // 1 render total ✅
});

// Opt out with flushSync
import { flushSync } from 'react-dom';
flushSync(() => setCount(c => c + 1));  // forces immediate render
flushSync(() => setFlag(f => !f));      // forces immediate render
```

---

### Q55. What is the React Compiler (React Forget)?

```
React Compiler = a build-time Babel plugin that:
  - Automatically memoizes components and values
  - Removes the need for manual useMemo / useCallback
  - Analyses data flow to insert optimizations at compile time

How it works:
  Input code:
    function Component({ items }) {
      const sorted = items.sort(); // expensive
      return <List data={sorted} />;
    }

  Compiled output (conceptually):
    function Component({ items }) {
      const sorted = useMemo(() => items.sort(), [items]);
      return <MemoizedList data={sorted} />;
    }
```

**Status (2025):** Available as opt-in, used in production at Meta.  
Install: `npm install babel-plugin-react-compiler`

---

### Q56. What is the `use()` hook? (React 19)

```jsx
import { use, Suspense } from 'react';

// Reading a Promise — integrates with Suspense
function UserProfile({ userPromise }) {
  const user = use(userPromise);  // suspends until resolved
  return <h1>{user.name}</h1>;
}

// Parent wraps with Suspense
function App() {
  const userPromise = fetchUser(1);  // start fetching early!
  return (
    <Suspense fallback={<Spinner />}>
      <UserProfile userPromise={userPromise} />
    </Suspense>
  );
}

// Reading Context — can be conditional unlike useContext
function Button({ isAdmin }) {
  if (isAdmin) {
    const { theme } = use(ThemeContext);  // conditional — unique to use()!
    return <button className={theme}>Admin Action</button>;
  }
  return <button>Regular</button>;
}
```

---

### Q57. Explain the complete rendering lifecycle in React 18

```
1. TRIGGER
   └─ setState() / new props / forceUpdate()

2. RENDER PHASE (pure, no side effects)
   └─ React calls your component function
   └─ Returns new virtual DOM
   └─ Diffs with previous virtual DOM
   └─ Can be interrupted (concurrent mode)

3. COMMIT PHASE (can't be interrupted)
   ├─ Before Mutation: useLayoutEffect cleanups run
   ├─ Mutation: React updates real DOM
   ├─ Layout: useLayoutEffect runs
   └─ Passive: useEffect runs (async, after paint)

4. BROWSER PAINTS (user sees changes)

5. PASSIVE EFFECTS
   └─ useEffect callbacks run
   └─ useEffect cleanups from previous render run
```

---

### Q58. What is Hydration in React?

Hydration = attaching React event handlers to **server-rendered HTML**.

```
Server:
  1. Renders React → HTML string
  2. Sends HTML to browser
  3. User sees content immediately (even before JS loads)

Browser:
  1. HTML displays (fast — no JS needed)
  2. JS bundle downloads
  3. React "hydrates" — attaches event handlers to existing HTML
  4. App becomes interactive

// Next.js hydration
// Server sends: <button>0</button>
// React hydrates: <button onClick={...}>0</button>
```

**Hydration errors** happen when server HTML doesn't match what React would render on client:
```jsx
// ❌ Hydration mismatch — Math.random() differs server vs client
function Component() {
  return <div>{Math.random()}</div>;
}

// ✅ Fix — use useEffect for client-only values
function Component() {
  const [random, setRandom] = useState(null);
  useEffect(() => setRandom(Math.random()), []);
  return <div>{random ?? 'loading...'}</div>;
}
```

---

### Q59. What are common React performance anti-patterns?

```jsx
// Anti-pattern 1: Anonymous functions in JSX
// ❌ New function object every render
<button onClick={() => handleClick(id)}>Click</button>
// ✅ useCallback
const handleClick = useCallback(() => doThing(id), [id]);

// Anti-pattern 2: Object literals in props
// ❌ New object every render → child always re-renders
<Component style={{ color: 'red' }} />
// ✅ Define outside or useMemo
const style = { color: 'red' }; // outside component
<Component style={style} />

// Anti-pattern 3: Index as key in dynamic lists
// ❌ Wrong matches when list changes
{items.map((item, i) => <Item key={i} {...item} />)}
// ✅ Stable unique ID
{items.map(item => <Item key={item.id} {...item} />)}

// Anti-pattern 4: useEffect without cleanup
// ❌ Memory leak
useEffect(() => {
  window.addEventListener('resize', handleResize);
}, []);
// ✅ Clean up
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);

// Anti-pattern 5: Derived state in useState
// ❌ Extra state that's just computed from other state
const [items, setItems] = useState([]);
const [count, setCount] = useState(0); // unnecessary!

// ✅ Derive directly — no extra state
const count = items.length;
```

---

### Q60. What is the difference between `useEffect` and server-side code?

```jsx
// useEffect NEVER runs on server (SSR)
// Only runs in browser

// Safe patterns:
useEffect(() => {
  // ✅ Browser-only APIs are safe inside useEffect
  localStorage.setItem('key', value);
  window.scrollTo(0, 0);
  document.title = 'New Title';
}, [value]);

// The problem — accessing browser APIs during render (SSR crashes)
function Component() {
  // ❌ localStorage is undefined on server!
  const theme = localStorage.getItem('theme');
  return <div className={theme}>...</div>;
}

// ✅ Fix — wrap in useEffect or check for window
function Component() {
  const [theme, setTheme] = useState('light');
  useEffect(() => {
    const saved = localStorage.getItem('theme');
    if (saved) setTheme(saved);
  }, []);
  return <div className={theme}>...</div>;
}
```

---

## 📊 Quick Cheatsheet

---

### All Hooks Summary

| Hook | Purpose | Key Rule |
|---|---|---|
| `useState` | Local state | Triggers re-render |
| `useEffect` | Side effects | Runs after render |
| `useRef` | DOM access / persist value | Does NOT re-render |
| `useContext` | Read context | Re-renders on context change |
| `useMemo` | Cache computed value | Avoid expensive recalculations |
| `useCallback` | Cache function reference | Use with React.memo children |
| `useReducer` | Complex state logic | Like local Redux |
| `useLayoutEffect` | DOM measurements | Runs before paint |
| `useTransition` | Non-urgent state update | React 18+ |
| `useDeferredValue` | Non-urgent value | React 18+ |
| `useOptimistic` | Instant UI feedback | React 19+ |
| `useActionState` | Async form state | React 19+ |
| `use()` | Read Promise/Context | Can be conditional — React 19+ |

---

### Common Interview Comparison Table

| Question | A | B |
|---|---|---|
| Props vs State | From parent, read-only | Own data, can change |
| `useMemo` vs `useCallback` | Caches a value | Caches a function |
| `useEffect` vs `useLayoutEffect` | After paint | Before paint |
| Controlled vs Uncontrolled | React drives value | DOM drives value |
| `useState` vs `useReducer` | Simple values | Complex logic |
| Context vs Redux | Simple sharing | Complex state machine |
| `Link` vs `NavLink` | Basic nav | Nav with active state |
| Server vs Client Component | Zero JS shipped | Interactive |

---

### When To Use What

```
Simple local value           → useState
Complex related values       → useReducer
Share across few components  → Lift state up + props
Share across many components → Context API
Complex app state            → Redux Toolkit or Zustand
DOM access                   → useRef
Cached computation           → useMemo
Stable function reference    → useCallback
Data fetch on mount          → useEffect(fn, [])
Data fetch on value change   → useEffect(fn, [value])
Non-urgent update            → useTransition
Deferred value               → useDeferredValue
Instant UI before server     → useOptimistic (React 19)
Async form state             → useActionState (React 19)
```

---

### Interview Answer Framework

Structure every answer using this:

```
1. WHAT   — define it simply (1 sentence)
2. WHY    — what problem does it solve
3. HOW    — quick code example
4. WHEN   — when to use / when NOT to use
5. GOTCHA — common mistake or edge case
```

---

*React 16.8 (Hooks) → React 18 (Concurrent) → React 19 (Actions)*  
*Updated: 2025-26 | 80+ Questions — Easy → Medium → Hard*
