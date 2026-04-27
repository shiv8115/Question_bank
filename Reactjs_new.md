# React.js Interview Questions

> **Section:** React.js
> **Total Questions:** 55
> **Difficulty:** 40% Basic · 40% Intermediate · 20% Practical
> **Focus:** Hooks, state management, lifecycle, performance, real-world patterns

---

## Table of Contents
- [Basic Questions (Q1–Q22)](#basic-questions)
- [Intermediate Questions (Q23–Q44)](#intermediate-questions)
- [Practical / Scenario Questions (Q45–Q55)](#practical--scenario-questions)

---

## Basic Questions

---

## Q1. What is React and what problem does it solve?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is React? Why was it created?

### Answer
React is a JavaScript **UI library** (not a framework) by Meta for building component-based user interfaces. It solves the problem of efficiently updating the DOM when application state changes, using a **Virtual DOM** to minimize costly real DOM operations.

### Explanation
Before React, updating the DOM directly (jQuery, vanilla JS) was error-prone and slow at scale. React introduces a declarative model: you describe *what* the UI should look like, React figures out *how* to update it.

---

## Q2. What is JSX?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is JSX and what does it compile to?

### Answer
JSX is a syntax extension that lets you write **HTML-like code inside JavaScript**. Babel compiles it to `React.createElement()` calls.

### Code
```jsx
// JSX
const element = <h1 className="title">Hello, {name}!</h1>;

// Compiled output
const element = React.createElement(
  "h1",
  { className: "title" },
  `Hello, ${name}!`
);
```

---

## Q3. What is the Virtual DOM?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is the Virtual DOM and how does React use it?

### Answer
The Virtual DOM is a **lightweight in-memory representation** of the real DOM. When state changes, React:
1. Creates a new Virtual DOM tree
2. Diffs it against the previous tree (**reconciliation**)
3. Computes the minimal changes needed
4. Applies only those changes to the real DOM (**commit phase**)

### Explanation
Direct DOM manipulation is slow. By batching and minimizing real DOM updates, React achieves better performance, especially in complex UIs.

---

## Q4. What is a React component?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is a component in React? What are the two types?

### Answer
A component is a **reusable, self-contained piece of UI**. Two types:
- **Function components** — plain JS functions returning JSX (modern standard)
- **Class components** — ES6 classes extending `React.Component` (legacy)

### Code
```jsx
// Function component (preferred)
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Class component (legacy)
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

---

## Q5. What are props?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What are props in React? Are they mutable?

### Answer
Props (properties) are **read-only inputs** passed from a parent component to a child. They are **immutable** — a component must never modify its own props.

### Code
```jsx
function UserCard({ name, age, onDelete }) {
  return (
    <div>
      <p>{name} ({age})</p>
      <button onClick={() => onDelete(name)}>Delete</button>
    </div>
  );
}

// Parent passes props
<UserCard name="Alice" age={30} onDelete={handleDelete} />

// Default props
function Button({ label = "Click Me", variant = "primary" }) {
  return <button className={variant}>{label}</button>;
}
```

---

## Q6. What is state in React?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is state? How does it differ from props?

### Answer
State is **mutable data local to a component**. When state changes, React re-renders the component. Props come from outside (parent); state is managed inside the component.

### Code
```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // initial value = 0

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(prev => prev - 1)}>-</button>
    </div>
  );
}
```

---

## Q7. What is `useState`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
How does `useState` work? What does it return?

### Answer
`useState(initialValue)` returns a **tuple**: `[currentValue, setterFunction]`. Calling the setter triggers a re-render with the new value.

### Code
```jsx
const [name, setName] = useState("Alice");
const [user, setUser] = useState({ name: "Alice", age: 30 });
const [items, setItems] = useState([]);

// Update object state — always spread to avoid mutation
setUser(prev => ({ ...prev, age: 31 }));

// Update array state
setItems(prev => [...prev, newItem]);
setItems(prev => prev.filter(item => item.id !== id));
```

---

## Q8. What is `useEffect`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is `useEffect` used for? Explain the dependency array.

### Answer
`useEffect` runs **side effects** after render: data fetching, subscriptions, DOM manipulation. The dependency array controls when it runs:
- `[]` — runs once after mount
- `[dep1, dep2]` — runs when those values change
- No array — runs after every render

### Code
```jsx
useEffect(() => {
  // Runs once (mount)
}, []);

useEffect(() => {
  // Runs when userId changes
  fetchUser(userId);
}, [userId]);

useEffect(() => {
  const sub = subscribe(topic);
  return () => sub.unsubscribe(); // cleanup on unmount
}, [topic]);
```

---

## Q9. What is `useRef`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is `useRef`? What are its two main use cases?

### Answer
`useRef` returns a mutable object `{ current: value }` that **persists across renders without causing re-renders**.

**Use cases:**
1. Accessing/manipulating DOM elements directly
2. Storing mutable values that shouldn't trigger re-renders (timers, previous values)

### Code
```jsx
// DOM access
function AutoFocusInput() {
  const inputRef = useRef(null);
  useEffect(() => inputRef.current.focus(), []);
  return <input ref={inputRef} />;
}

// Mutable value (timer ID)
function Timer() {
  const timerRef = useRef(null);
  const start = () => {
    timerRef.current = setInterval(() => console.log("tick"), 1000);
  };
  const stop = () => clearInterval(timerRef.current);
  return <><button onClick={start}>Start</button><button onClick={stop}>Stop</button></>;
}
```

---

## Q10. What is `useContext`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is React Context and `useContext`? When would you use it?

### Answer
Context provides a way to **pass data through the component tree** without prop drilling. `useContext` consumes the nearest matching context.

### Code
```jsx
// Create context
const ThemeContext = createContext("light");

// Provide it
function App() {
  const [theme, setTheme] = useState("light");
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <MainLayout />
    </ThemeContext.Provider>
  );
}

// Consume it (anywhere in the tree)
function Button() {
  const { theme, setTheme } = useContext(ThemeContext);
  return (
    <button className={theme} onClick={() => setTheme("dark")}>
      Toggle
    </button>
  );
}
```

---

## Q11. What is the component lifecycle in function components?
**Difficulty:** Basic | **Type:** Conceptual

### Question
How do you replicate class component lifecycle methods using hooks?

### Answer
| Class lifecycle          | Hooks equivalent                     |
|--------------------------|--------------------------------------|
| `componentDidMount`      | `useEffect(() => {}, [])`            |
| `componentDidUpdate`     | `useEffect(() => {}, [dep])`         |
| `componentWillUnmount`   | Cleanup function in `useEffect`      |
| `shouldComponentUpdate`  | `React.memo` + `useMemo`             |

### Code
```jsx
function Component({ id }) {
  // Mount
  useEffect(() => {
    console.log("Mounted");
    return () => console.log("Unmounted"); // cleanup
  }, []);

  // Update (when id changes)
  useEffect(() => {
    fetchData(id);
  }, [id]);
}
```

---

## Q12. What is conditional rendering in React?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What are the ways to conditionally render content in React?

### Answer
Three common patterns: ternary operator, logical `&&`, or early return.

### Code
```jsx
function Dashboard({ user, isLoading, error }) {
  // Early return
  if (isLoading) return <Spinner />;
  if (error) return <ErrorMessage message={error} />;

  return (
    <div>
      {/* Ternary */}
      {user ? <UserProfile user={user} /> : <GuestBanner />}

      {/* Logical AND (only renders when truthy) */}
      {user?.isAdmin && <AdminPanel />}

      {/* Nullish coalescing for defaults */}
      <p>{user?.bio ?? "No bio provided"}</p>
    </div>
  );
}
```

---

## Q13. How do you render lists in React?
**Difficulty:** Basic | **Type:** Conceptual

### Question
How do you render a list of items? Why is the `key` prop important?

### Answer
Use `.map()` to render lists. Each item needs a unique `key` prop to help React identify which items changed, were added, or removed.

### Code
```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}> {/* key must be unique & stable */}
          {user.name} — {user.email}
        </li>
      ))}
    </ul>
  );
}

// ❌ Avoid using index as key (causes bugs with reordering)
{items.map((item, index) => <li key={index}>{item}</li>)}

// ✅ Use stable unique IDs
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

---

## Q14. What is event handling in React?
**Difficulty:** Basic | **Type:** Conceptual

### Question
How does event handling work in React? How is it different from HTML?

### Answer
React uses **synthetic events** — a cross-browser wrapper. Differences from HTML:
- Event names are camelCase (`onClick` not `onclick`)
- Pass function references, not strings
- `e.preventDefault()` works the same way

### Code
```jsx
function Form() {
  const [value, setValue] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault(); // prevent page reload
    console.log("Submitted:", value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={value}
        onChange={(e) => setValue(e.target.value)}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

---

## Q15. What is `React.Fragment`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
Why do React components need a single root element? How does Fragment help?

### Answer
JSX requires a single root. Wrapping in `<div>` adds unnecessary DOM nodes. `<Fragment>` (or `<>`) groups elements without adding DOM nodes.

### Code
```jsx
// ❌ Adds unnecessary div to DOM
return (
  <div>
    <td>Cell 1</td>
    <td>Cell 2</td>
  </div>
);

// ✅ Fragment — no extra DOM node
return (
  <>
    <td>Cell 1</td>
    <td>Cell 2</td>
  </>
);

// Named fragment (needed when you need a key)
return (
  <React.Fragment key={item.id}>
    <dt>{item.term}</dt>
    <dd>{item.description}</dd>
  </React.Fragment>
);
```

---

## Q16. What is controlled vs uncontrolled components?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is the difference between controlled and uncontrolled form inputs?

### Answer
- **Controlled**: React state drives the input value. Single source of truth.
- **Uncontrolled**: DOM manages the value; React reads it via `ref`.

### Code
```jsx
// Controlled — React owns the value
function ControlledInput() {
  const [value, setValue] = useState("");
  return <input value={value} onChange={e => setValue(e.target.value)} />;
}

// Uncontrolled — DOM owns the value
function UncontrolledInput() {
  const inputRef = useRef();
  const handleSubmit = () => console.log(inputRef.current.value);
  return (
    <>
      <input ref={inputRef} defaultValue="initial" />
      <button onClick={handleSubmit}>Get Value</button>
    </>
  );
}
```

---

## Q17. What is prop drilling and why is it a problem?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is prop drilling? How do you avoid it?

### Answer
Prop drilling is passing props through many intermediate components that don't use the data — just pass it down. Solutions: **Context API**, **state management libraries** (Redux, Zustand), or **component composition**.

### Code
```jsx
// ❌ Prop drilling — Middle doesn't use user, just passes it
function App() {
  const user = { name: "Alice" };
  return <Middle user={user} />;
}
function Middle({ user }) {
  return <Deep user={user} />;  // doesn't use user itself
}
function Deep({ user }) {
  return <p>{user.name}</p>;
}

// ✅ Context — Deep consumes directly
const UserContext = createContext();
function App() {
  return (
    <UserContext.Provider value={{ name: "Alice" }}>
      <Middle />
    </UserContext.Provider>
  );
}
function Deep() {
  const user = useContext(UserContext);
  return <p>{user.name}</p>;
}
```

---

## Q18. What is `React.memo`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What does `React.memo` do?

### Answer
`React.memo` is a HOC that **memoizes a component** — it skips re-rendering if props haven't changed (shallow comparison). Useful for expensive child components.

### Code
```jsx
// Without memo — re-renders every time parent renders
function ExpensiveList({ items }) {
  return items.map(item => <div key={item.id}>{item.name}</div>);
}

// With memo — only re-renders when items changes
const ExpensiveList = React.memo(function({ items }) {
  return items.map(item => <div key={item.id}>{item.name}</div>);
});

// Custom comparison
const Component = React.memo(MyComponent, (prevProps, nextProps) => {
  return prevProps.id === nextProps.id; // true = skip re-render
});
```

---

## Q19. What are React keys and why are they important?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What happens if you don't provide keys, or use array indexes as keys?

### Answer
Without keys, React re-renders the entire list. With unstable keys (indexes), React can't track reordering correctly — causing state bugs and performance issues.

### Explanation
React uses keys to match old and new elements during reconciliation. A stable, unique key lets React reuse and move components correctly instead of destroying and recreating them.

---

## Q20. What is the difference between `useEffect` and `useLayoutEffect`?
**Difficulty:** Basic | **Type:** Conceptual

### Question
When would you use `useLayoutEffect` instead of `useEffect`?

### Answer
- `useEffect` — runs **after** the browser has painted. Async. Use for most effects.
- `useLayoutEffect` — runs **synchronously after DOM mutations**, before painting. Use when you need to measure DOM or prevent visual flicker.

### Code
```jsx
// useLayoutEffect for DOM measurements (prevents flicker)
function Tooltip({ targetRef }) {
  const tooltipRef = useRef();
  useLayoutEffect(() => {
    const { top } = targetRef.current.getBoundingClientRect();
    tooltipRef.current.style.top = `${top - 30}px`; // before paint
  });
  return <div ref={tooltipRef} className="tooltip" />;
}
```

---

## Q21. What is `children` prop?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is the `children` prop? How do you use it?

### Answer
`children` is a special prop that represents **whatever is passed between a component's opening and closing tags**.

### Code
```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-body">{children}</div>
    </div>
  );
}

// Usage
<Card title="User Info">
  <p>Name: Alice</p>
  <p>Age: 30</p>
  <button>Edit</button>
</Card>
```

---

## Q22. What is a pure component?
**Difficulty:** Basic | **Type:** Conceptual

### Question
What is a pure component in React?

### Answer
A pure component **re-renders only when its props or state change**. In function components, this is achieved with `React.memo`. In class components, `PureComponent` does shallow prop/state comparison.

### Explanation
Regular components re-render whenever the parent renders. Pure components add a performance optimization by skipping re-renders when inputs haven't changed.

---

## Intermediate Questions

---

## Q23. What is `useReducer`? When would you use it over `useState`?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
Explain `useReducer`. When does it make more sense than `useState`?

### Answer
`useReducer(reducer, initialState)` manages complex state transitions through a **reducer function**. Prefer it when:
- Next state depends on previous state in complex ways
- Multiple sub-values in state
- State logic is complex enough to test in isolation

### Code
```jsx
const initialState = { count: 0, loading: false, error: null };

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT": return { ...state, count: state.count + 1 };
    case "DECREMENT": return { ...state, count: state.count - 1 };
    case "RESET":     return initialState;
    case "SET_LOADING": return { ...state, loading: action.payload };
    default: throw new Error(`Unknown action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <div>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+</button>
      <button onClick={() => dispatch({ type: "RESET" })}>Reset</button>
    </div>
  );
}
```

---

## Q24. What is `useMemo`?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What does `useMemo` do? When should you use it?

### Answer
`useMemo` memoizes the **result of a computation** — it only recalculates when dependencies change. Use for expensive calculations, not for every value.

### Code
```jsx
function ProductList({ products, filterText }) {
  // Without useMemo — recalculates on every render
  const filtered = products.filter(p => p.name.includes(filterText));

  // With useMemo — only recalculates when products or filterText change
  const filtered = useMemo(
    () => products.filter(p => p.name.includes(filterText)),
    [products, filterText]
  );

  // Also useful for expensive derived data
  const stats = useMemo(() => ({
    total: products.length,
    avgPrice: products.reduce((s, p) => s + p.price, 0) / products.length
  }), [products]);

  return <ul>{filtered.map(p => <li key={p.id}>{p.name}</li>)}</ul>;
}
```

---

## Q25. What is `useCallback`?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is `useCallback`? How does it differ from `useMemo`?

### Answer
`useCallback` memoizes a **function reference** — returns the same function instance unless dependencies change. Used to prevent child components from re-rendering due to new function references on every render.

- `useMemo` → memoizes a **value** (result of calling a function)
- `useCallback` → memoizes a **function** (the function itself)

### Code
```jsx
function Parent() {
  const [count, setCount] = useState(0);

  // ❌ New function reference every render → Child always re-renders
  const handleClick = () => console.log("clicked");

  // ✅ Same reference unless deps change → Child skips re-render
  const handleClick = useCallback(() => {
    console.log("clicked", count);
  }, [count]);

  return <MemoizedChild onClick={handleClick} />;
}

const MemoizedChild = React.memo(({ onClick }) => {
  console.log("Child rendered");
  return <button onClick={onClick}>Click</button>;
});
```

---

## Q26. What is a custom hook?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is a custom hook? Build a `useFetch` example.

### Answer
A custom hook is a **JavaScript function starting with `use`** that can call other hooks. It extracts reusable stateful logic from components.

### Code
```jsx
// Custom hook
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;
    setLoading(true);

    fetch(url)
      .then(res => {
        if (!res.ok) throw new Error(`HTTP ${res.status}`);
        return res.json();
      })
      .then(data => { if (!cancelled) setData(data); })
      .catch(err => { if (!cancelled) setError(err.message); })
      .finally(() => { if (!cancelled) setLoading(false); });

    return () => { cancelled = true; }; // cleanup for race conditions
  }, [url]);

  return { data, loading, error };
}

// Usage
function UserProfile({ id }) {
  const { data: user, loading, error } = useFetch(`/api/users/${id}`);
  if (loading) return <Spinner />;
  if (error) return <p>Error: {error}</p>;
  return <h1>{user.name}</h1>;
}
```

---

## Q27. What is React's reconciliation algorithm?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
How does React decide what to update in the DOM?

### Answer
React's reconciliation uses two heuristics:
1. **Different element type** → destroy old tree, build new one
2. **Same element type** → update attributes, recurse on children
3. **Keys** → match children across renders to reorder instead of recreate

The current algorithm is called **Fiber**, which enables incremental rendering and prioritization.

---

## Q28. What are React Portals?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What are React Portals? When would you use them?

### Answer
Portals render a component **outside its parent DOM hierarchy** while keeping it in the React tree. Used for modals, tooltips, dropdowns — anything that needs to visually escape a container (overflow: hidden, z-index issues).

### Code
```jsx
import { createPortal } from "react-dom";

function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;

  return createPortal(
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal-content" onClick={e => e.stopPropagation()}>
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>,
    document.getElementById("modal-root") // renders outside #app
  );
}
```

---

## Q29. What is the Context API's limitation? When should you use a state manager instead?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is wrong with using Context for all global state?

### Answer
**Context limitation**: Every component that consumes a context re-renders when **any** value in that context changes — even if they only care about one field. This causes unnecessary re-renders at scale.

**Use state managers** (Redux, Zustand, Jotai) when you have:
- Frequently changing state (counters, real-time data)
- Large number of consumers
- Need for middleware, time-travel debugging

**Context is fine for**: theme, locale, auth user, rarely-changing config.

---

## Q30. What is error boundary in React?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is an error boundary? How do you implement one?

### Answer
Error boundaries are **class components** that catch JavaScript errors in their child tree and display a fallback UI instead of crashing the whole app. (No function component equivalent yet.)

### Code
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, info) {
    logErrorToService(error, info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

// Usage
<ErrorBoundary fallback={<ErrorPage />}>
  <Dashboard />
</ErrorBoundary>
```

---

## Q31. What is lazy loading in React?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
How do you implement code splitting and lazy loading in React?

### Answer
`React.lazy()` dynamically imports a component only when it's needed. Pair with `<Suspense>` for a loading fallback.

### Code
```jsx
import { lazy, Suspense } from "react";

// Only loads AdminDashboard JS bundle when route is visited
const AdminDashboard = lazy(() => import("./AdminDashboard"));
const UserProfile = lazy(() => import("./UserProfile"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/admin" element={<AdminDashboard />} />
        <Route path="/profile" element={<UserProfile />} />
      </Routes>
    </Suspense>
  );
}
```

---

## Q32. What is `useImperativeHandle` and `forwardRef`?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is `forwardRef`? When would you use `useImperativeHandle`?

### Answer
`forwardRef` lets a parent pass a `ref` to a **child's DOM node**. `useImperativeHandle` lets you control what the ref **exposes** (instead of the raw DOM node).

### Code
```jsx
// forwardRef — parent gets access to child's input DOM node
const FancyInput = forwardRef(function({ label }, ref) {
  return (
    <div>
      <label>{label}</label>
      <input ref={ref} />
    </div>
  );
});

// useImperativeHandle — expose custom API
const VideoPlayer = forwardRef(function(props, ref) {
  const videoRef = useRef();

  useImperativeHandle(ref, () => ({
    play: () => videoRef.current.play(),
    pause: () => videoRef.current.pause(),
  }));

  return <video ref={videoRef} src={props.src} />;
});

// Parent usage
function App() {
  const playerRef = useRef();
  return (
    <>
      <VideoPlayer ref={playerRef} src="/video.mp4" />
      <button onClick={() => playerRef.current.play()}>Play</button>
    </>
  );
}
```

---

## Q33. What is the `useId` hook?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is `useId` and why was it introduced?

### Answer
`useId` generates **stable, unique IDs** that are consistent between server and client renders, solving hydration mismatches in SSR apps when connecting labels to form inputs.

### Code
```jsx
function FormField({ label }) {
  const id = useId();
  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input id={id} type="text" />
    </div>
  );
}
// Multiple instances get different IDs: :r0:, :r1:, etc.
```

---

## Q34. How does React batching work?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is state update batching? How did it change in React 18?

### Answer
React batches multiple `setState` calls into a **single re-render** for performance.

- **React 17**: Only batched inside React event handlers (not in `setTimeout`, Promises)
- **React 18**: **Automatic batching** everywhere, including async functions and native events

### Code
```jsx
// React 18 — all three updates batched into ONE re-render
setTimeout(() => {
  setCount(c => c + 1);
  setName("Bob");
  setVisible(true);
  // Previously: 3 re-renders in React 17
  // Now: 1 re-render in React 18
}, 1000);

// Opt out of batching (rare)
import { flushSync } from "react-dom";
flushSync(() => setCount(c => c + 1)); // forces immediate re-render
```

---

## Q35. What are render props?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is the render props pattern? When would you use it?

### Answer
Render props is a pattern where a component receives a **function as a prop** and calls it to render content, sharing stateful logic without HOCs.

### Code
```jsx
// Component with render prop
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });

  return (
    <div onMouseMove={e => setPos({ x: e.clientX, y: e.clientY })}>
      {render(pos)}
    </div>
  );
}

// Usage — consumer controls what to render
<MouseTracker render={({ x, y }) => (
  <p>Mouse at {x}, {y}</p>
)} />

// Modern alternative: custom hooks do this more cleanly
function useMouse() {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  useEffect(() => {
    const handler = e => setPos({ x: e.clientX, y: e.clientY });
    window.addEventListener("mousemove", handler);
    return () => window.removeEventListener("mousemove", handler);
  }, []);
  return pos;
}
```

---

## Q36. What are Higher-Order Components (HOC)?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What is a Higher-Order Component? Give an example.

### Answer
A HOC is a **function that takes a component and returns a new component** with enhanced behavior. Common for cross-cutting concerns like auth checks, logging, analytics.

### Code
```jsx
// HOC for authentication guard
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const { isAuthenticated, redirectTo } = useAuth();

    if (!isAuthenticated) {
      return <Navigate to="/login" />;
    }

    return <WrappedComponent {...props} />;
  };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);

// HOC for loading state
function withLoading(WrappedComponent) {
  return function({ isLoading, ...props }) {
    if (isLoading) return <Spinner />;
    return <WrappedComponent {...props} />;
  };
}
```

---

## Q37. What is the difference between `useEffect` cleanup and `AbortController`?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
Why do fetch requests in `useEffect` sometimes cause "state update on unmounted component" warnings? How do you fix it?

### Answer
When a component unmounts while a fetch is in-flight, the response arrives and tries to update state on an unmounted component. Fix with `AbortController` or a cancellation flag.

### Code
```jsx
useEffect(() => {
  const controller = new AbortController();

  async function loadData() {
    try {
      const res = await fetch(`/api/users/${id}`, {
        signal: controller.signal
      });
      const data = await res.json();
      setUser(data);
    } catch (err) {
      if (err.name !== "AbortError") setError(err.message);
    }
  }

  loadData();
  return () => controller.abort(); // cancel on unmount
}, [id]);
```

---

## Q38. How do you optimize a React app's performance?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
List at least 5 React performance optimization techniques.

### Answer
1. `React.memo` — skip re-renders for pure components
2. `useMemo` — memoize expensive calculations
3. `useCallback` — stable function references for memoized children
4. **Code splitting** with `React.lazy` + `Suspense`
5. **Virtualization** for long lists (`react-window`, `react-virtual`)
6. **Avoid inline objects/functions** in JSX (creates new refs each render)
7. **Lift state down** — don't put state higher than necessary
8. **Debounce** expensive operations (search, resize)
9. **React.StrictMode** — highlights issues in development
10. Use the **React Profiler** to find actual bottlenecks

---

## Q39. What is `Suspense` in React?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What does `<Suspense>` do beyond lazy loading?

### Answer
`Suspense` shows a fallback UI while its children are **waiting** — originally for lazy-loaded components, now extended to **data fetching** (with React Query, Relay, or React 18's use() hook).

### Code
```jsx
// With lazy loading
<Suspense fallback={<Spinner />}>
  <LazyComponent />
</Suspense>

// Nested Suspense (different loading states)
<Suspense fallback={<PageSkeleton />}>
  <Header />
  <Suspense fallback={<FeedSkeleton />}>
    <NewsFeed />
  </Suspense>
  <Footer />
</Suspense>

// React 18 — use() hook with Suspense for data
function UserName({ userPromise }) {
  const user = use(userPromise); // suspends until resolved
  return <h1>{user.name}</h1>;
}
```

---

## Q40. What is React's `StrictMode`?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What does `React.StrictMode` do?

### Answer
`StrictMode` is a development tool that **deliberately double-invokes** certain functions (render, useState initializer, useReducer reducer) to help detect side effects. It also warns about deprecated APIs. Has **no effect in production**.

### Code
```jsx
// In index.js or App.js
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// Effects of StrictMode:
// - Renders components twice (in dev) to detect impure renders
// - Double-invokes: render, state initializer, reducer, ref callback
// - Warns about legacy string refs, legacy Context API, findDOMNode
```

---

## Q41. What is the difference between `state` and a `ref` for storing values?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
When would you use `useRef` to store a value instead of `useState`?

### Answer
Use `useRef` when you need to **store a value that changes but shouldn't cause a re-render**:
- Timer IDs
- Previous prop/state values
- Mutable instance variables
- DOM node references

### Code
```jsx
function StopWatch() {
  const [time, setTime] = useState(0);
  const intervalRef = useRef(null); // doesn't cause re-render when changed

  const start = () => {
    intervalRef.current = setInterval(() => setTime(t => t + 1), 1000);
  };

  const stop = () => clearInterval(intervalRef.current);

  return (
    <div>
      <p>{time}s</p>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </div>
  );
}
```

---

## Q42. Explain the React component rendering flow.
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
Walk through what happens when a React component's state changes.

### Answer
1. **Trigger**: State/prop change or `forceUpdate`
2. **Render phase**: React calls the component function, builds new Virtual DOM tree (pure, no side effects)
3. **Reconciliation**: Diffs new vs old tree
4. **Commit phase**: Applies DOM mutations, runs `useLayoutEffect`
5. **Passive effects**: Runs `useEffect` after paint

---

## Q43. What is the `key` prop used for beyond lists?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
Can you use `key` outside of lists? Give a use case.

### Answer
Yes. Setting a `key` on a component **forces React to remount it** (destroy + recreate) when the key changes. This is a useful trick to reset all state in a component.

### Code
```jsx
// ❌ Problem: switching users doesn't reset the form state
function UserForm({ userId }) {
  const [name, setName] = useState("");
  // When userId changes, name doesn't reset
  return <input value={name} onChange={e => setName(e.target.value)} />;
}

// ✅ Fix: key forces full remount when userId changes
function App({ userId }) {
  return <UserForm key={userId} userId={userId} />;
  // All state in UserForm resets when userId changes
}
```

---

## Q44. What is `useTransition` in React 18?
**Difficulty:** Intermediate | **Type:** Conceptual

### Question
What problem does `useTransition` solve?

### Answer
`useTransition` marks state updates as **non-urgent**, letting React keep the UI responsive while heavy updates happen in the background. Urgent updates (typing) get priority over transitions (search results rendering).

### Code
```jsx
function SearchPage() {
  const [query, setQuery] = useState("");
  const [results, setResults] = useState([]);
  const [isPending, startTransition] = useTransition();

  const handleSearch = (e) => {
    setQuery(e.target.value); // urgent — updates input immediately

    startTransition(() => {
      setResults(heavySearch(e.target.value)); // non-urgent — can be interrupted
    });
  };

  return (
    <>
      <input value={query} onChange={handleSearch} />
      {isPending ? <Spinner /> : <ResultList results={results} />}
    </>
  );
}
```

---

## Practical / Scenario Questions

---

## Q45. A component is re-rendering too often. How do you debug and fix it?
**Difficulty:** Practical | **Type:** Debugging

### Question
Users report that a data table is sluggish. How do you find and fix the performance issue?

### Answer
**Debugging steps:**
1. Use **React DevTools Profiler** — record interactions, find components that render frequently
2. Enable "Highlight updates when components render"
3. Check for: inline objects/functions in JSX, missing `React.memo`, wrong `useEffect` deps

### Code
```jsx
// ❌ Problem — new object reference every render
function Parent() {
  const [count, setCount] = useState(0);
  const config = { theme: "dark" }; // new object each render!

  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <ExpensiveTable config={config} /> {/* re-renders on every count change */}
    </>
  );
}

// ✅ Fix 1 — move config outside component (if static)
const config = { theme: "dark" };

// ✅ Fix 2 — useMemo if config depends on state
const config = useMemo(() => ({ theme: "dark" }), []); // stable reference

// ✅ Fix 3 — wrap child in memo
const ExpensiveTable = React.memo(({ config }) => { /* ... */ });
```

---

## Q46. Build a debounced search input component.
**Difficulty:** Practical | **Type:** Coding

### Question
Create a `SearchInput` component that only fires an API call 300ms after the user stops typing.

### Answer

### Code
```jsx
import { useState, useEffect, useCallback } from "react";

function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}

function SearchInput({ onSearch }) {
  const [query, setQuery] = useState("");
  const debouncedQuery = useDebounce(query, 300);

  useEffect(() => {
    if (debouncedQuery) {
      onSearch(debouncedQuery);
    }
  }, [debouncedQuery, onSearch]);

  return (
    <div>
      <input
        type="search"
        value={query}
        onChange={e => setQuery(e.target.value)}
        placeholder="Search..."
      />
      {query !== debouncedQuery && <span>Searching...</span>}
    </div>
  );
}
```

---

## Q47. How would you implement infinite scrolling in React?
**Difficulty:** Practical | **Type:** Scenario / Coding

### Question
Implement an infinite scroll list that loads more items when the user nears the bottom.

### Answer

### Code
```jsx
function InfiniteList() {
  const [items, setItems] = useState([]);
  const [page, setPage] = useState(1);
  const [hasMore, setHasMore] = useState(true);
  const [loading, setLoading] = useState(false);
  const observerRef = useRef();

  const loadMore = useCallback(async () => {
    if (loading || !hasMore) return;
    setLoading(true);
    const newItems = await fetchItems(page);
    setItems(prev => [...prev, ...newItems]);
    setHasMore(newItems.length > 0);
    setPage(p => p + 1);
    setLoading(false);
  }, [page, loading, hasMore]);

  // Intersection Observer — watches a sentinel element at bottom
  const sentinelRef = useCallback(node => {
    if (observerRef.current) observerRef.current.disconnect();
    observerRef.current = new IntersectionObserver(entries => {
      if (entries[0].isIntersecting) loadMore();
    });
    if (node) observerRef.current.observe(node);
  }, [loadMore]);

  useEffect(() => { loadMore(); }, []); // initial load

  return (
    <div>
      {items.map(item => <div key={item.id}>{item.name}</div>)}
      {loading && <Spinner />}
      {!hasMore && <p>No more items</p>}
      <div ref={sentinelRef} /> {/* sentinel */}
    </div>
  );
}
```

---

## Q48. How do you manage forms with complex validation in React?
**Difficulty:** Practical | **Type:** Scenario

### Question
How would you build a registration form with validation without a library?

### Answer

### Code
```jsx
function RegistrationForm() {
  const [form, setForm] = useState({ email: "", password: "", age: "" });
  const [errors, setErrors] = useState({});
  const [submitted, setSubmitted] = useState(false);

  const validate = (values) => {
    const errs = {};
    if (!values.email.includes("@")) errs.email = "Invalid email";
    if (values.password.length < 8) errs.password = "Min 8 characters";
    if (Number(values.age) < 18) errs.age = "Must be 18+";
    return errs;
  };

  const handleChange = (e) => {
    const updated = { ...form, [e.target.name]: e.target.value };
    setForm(updated);
    if (submitted) setErrors(validate(updated)); // live validation after first submit
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    setSubmitted(true);
    const errs = validate(form);
    setErrors(errs);
    if (Object.keys(errs).length === 0) {
      submitToAPI(form);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" value={form.email} onChange={handleChange} />
      {errors.email && <span className="error">{errors.email}</span>}
      {/* similar for password, age */}
      <button type="submit">Register</button>
    </form>
  );
}
```

---

## Q49. Explain how you'd implement a global loading/toast notification system.
**Difficulty:** Practical | **Type:** Scenario

### Question
How would you build a toast notification system in React without a library?

### Answer

### Code
```jsx
// Context + Portal approach
const ToastContext = createContext();

function ToastProvider({ children }) {
  const [toasts, setToasts] = useState([]);

  const addToast = useCallback((message, type = "info", duration = 3000) => {
    const id = Date.now();
    setToasts(prev => [...prev, { id, message, type }]);
    setTimeout(() => setToasts(prev => prev.filter(t => t.id !== id)), duration);
  }, []);

  return (
    <ToastContext.Provider value={{ addToast }}>
      {children}
      {createPortal(
        <div className="toast-container">
          {toasts.map(toast => (
            <div key={toast.id} className={`toast toast-${toast.type}`}>
              {toast.message}
            </div>
          ))}
        </div>,
        document.body
      )}
    </ToastContext.Provider>
  );
}

export const useToast = () => useContext(ToastContext);

// Usage anywhere in the app
function SaveButton() {
  const { addToast } = useToast();
  const handleSave = async () => {
    try {
      await saveData();
      addToast("Saved successfully!", "success");
    } catch {
      addToast("Save failed!", "error");
    }
  };
  return <button onClick={handleSave}>Save</button>;
}
```

---

## Q50. What is wrong with this code?
**Difficulty:** Practical | **Type:** Debugging

### Question
Find the bug(s) in this component:
```jsx
function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function load() {
      const data = await fetchUsers();
      setUsers(data);
    }
    load();
  }, [users]); // ← dependency

  return <ul>{users.map(u => <li>{u.name}</li>)}</ul>;
}
```

### Answer
**Two bugs:**
1. `users` in the dependency array causes an **infinite loop**: fetch → setUsers → re-render → effect runs again (users changed) → fetch again...
2. Missing `key` prop on `<li>` elements

### Code
```jsx
// ✅ Fixed
function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function load() {
      const data = await fetchUsers();
      setUsers(data);
    }
    load();
  }, []); // ← empty array: run once on mount

  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)} {/* ← key added */}
    </ul>
  );
}
```

---

## Q51–Q55. Quick-Fire Practical Scenarios
**Difficulty:** Practical | **Type:** Mixed

### Q51. How do you share logic between two components that both need pagination state?
**Answer:** Extract into a `usePagination` custom hook that returns `{ page, totalPages, nextPage, prevPage, setPage }`. Both components call the hook independently (each gets their own state).

---

### Q52. A user reports a stale value inside a `useEffect`. What's the likely cause?
**Answer:** **Stale closure** — the effect captured an old value of a variable but that variable isn't in the dependency array. Fix: add the variable to the deps array, or use a `ref` to always have the latest value.

```jsx
// Stale closure bug
const countRef = useRef(count);
countRef.current = count; // always latest
useEffect(() => {
  const timer = setInterval(() => {
    console.log(countRef.current); // always fresh
  }, 1000);
  return () => clearInterval(timer);
}, []); // no stale closure
```

---

### Q53. How would you implement dark mode toggle that persists across refreshes?
**Answer:** Store preference in `localStorage`, initialize state from it, update on toggle.

```jsx
function useDarkMode() {
  const [isDark, setIsDark] = useState(
    () => localStorage.getItem("theme") === "dark"
  );
  const toggle = () => {
    setIsDark(prev => {
      localStorage.setItem("theme", !prev ? "dark" : "light");
      return !prev;
    });
  };
  return [isDark, toggle];
}
```

---

### Q54. You need to call an API only once per app session, not per component mount. How?
**Answer:** Fetch outside React (module-level singleton with a promise cache), or use a shared context/store initialized at app root with `useEffect(() => {}, [])`.

```jsx
// Module-level cache
let cachedConfig = null;
async function getConfig() {
  if (!cachedConfig) cachedConfig = fetch("/api/config").then(r => r.json());
  return cachedConfig;
}
```

---

### Q55. A child component needs to trigger a function in its parent. How do you do this?
**Answer:** Pass a callback function as a prop from parent to child. The child calls it when needed. For deeply nested components, use Context or state management.

```jsx
function Parent() {
  const handleChildAction = (data) => {
    console.log("Child sent:", data);
  };
  return <Child onAction={handleChildAction} />;
}

function Child({ onAction }) {
  return <button onClick={() => onAction({ id: 1 })}>Send to Parent</button>;
}
```

---

*End of React Questions — 55 questions total*
*Next: → [Node.js Questions](../nodejs/questions.md)*
