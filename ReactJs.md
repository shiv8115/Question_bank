# React.js Interview Questions Bank
**Chunk 3 of 6** | **70 Questions Total** | **Easy → Intermediate → Advanced**

---

## 🟢 EASY LEVEL (Questions 1-25)

### **Q1: What is React and what are its key features?**
**Answer:** React is a JavaScript library for building user interfaces, particularly web applications. Key features:
- **Virtual DOM** for efficient updates
- **Component-based** architecture
- **Unidirectional data flow**
- **JSX** syntax
- **Reusable components**

### **Q2: What is JSX?**
**Answer:** JSX (JavaScript XML) is a syntax extension that allows writing HTML-like code in JavaScript:
```jsx
const element = <h1>Hello, World!</h1>;
const component = <MyComponent prop="value" />;
```

### **Q3: What is the difference between functional and class components?**
**Answer:**
```jsx
// Functional Component
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Class Component
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### **Q4: What are props in React?**
**Answer:** Props (properties) are read-only inputs passed from parent to child components:
```jsx
function Child({ name, age }) {
  return <div>{name} is {age} years old</div>;
}

function Parent() {
  return <Child name="John" age={25} />;
}
```

### **Q5: What is state in React?**
**Answer:** State is mutable data that belongs to a component and can trigger re-renders when updated:
```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### **Q6: What is the useState hook?**
**Answer:** useState is a hook that allows functional components to have state:
```jsx
const [state, setState] = useState(initialValue);
```

### **Q7: How do you handle events in React?**
**Answer:**
```jsx
function Button() {
  const handleClick = (e) => {
    e.preventDefault();
    console.log('Button clicked');
  };
  
  return <button onClick={handleClick}>Click me</button>;
}
```

### **Q8: What is the useEffect hook?**
**Answer:** useEffect performs side effects in functional components:
```jsx
useEffect(() => {
  // Side effect code
  return () => {
    // Cleanup code
  };
}, [dependencies]);
```

### **Q9: How do you conditionally render elements in React?**
**Answer:**
```jsx
function Greeting({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please log in</h1>}
      {isLoggedIn && <UserDashboard />}
    </div>
  );
}
```

### **Q10: How do you render lists in React?**
**Answer:**
```jsx
function ItemList({ items }) {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

### **Q11: What are keys in React and why are they important?**
**Answer:** Keys help React identify which items have changed, been added, or removed:
```jsx
// Good - unique stable key
{items.map(item => <Item key={item.id} data={item} />)}

// Bad - index as key (for dynamic lists)
{items.map((item, index) => <Item key={index} data={item} />)}
```

### **Q12: How do you handle forms in React?**
**Answer:**
```jsx
function ContactForm() {
  const [formData, setFormData] = useState({ name: '', email: '' });
  
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(formData);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={formData.name} onChange={handleChange} />
      <input name="email" value={formData.email} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### **Q13: What is the difference between controlled and uncontrolled components?**
**Answer:**
- **Controlled:** Component's value is controlled by React state
- **Uncontrolled:** Component maintains its own internal state

```jsx
// Controlled
const [value, setValue] = useState('');
<input value={value} onChange={e => setValue(e.target.value)} />

// Uncontrolled
const inputRef = useRef();
<input ref={inputRef} defaultValue="initial" />
```

### **Q14: What is the useRef hook?**
**Answer:** useRef creates a mutable ref object that persists across renders:
```jsx
function TextInput() {
  const inputRef = useRef(null);
  
  const focusInput = () => {
    inputRef.current.focus();
  };
  
  return (
    <div>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

### **Q15: How do you pass data from child to parent component?**
**Answer:** Pass a callback function from parent to child:
```jsx
function Parent() {
  const handleChildData = (data) => {
    console.log('Received from child:', data);
  };
  
  return <Child onData={handleChildData} />;
}

function Child({ onData }) {
  return (
    <button onClick={() => onData('Hello Parent!')}>
      Send Data
    </button>
  );
}
```

### **Q16: What is component composition?**
**Answer:** Building complex components by combining simpler ones:
```jsx
function Card({ children, title }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
}

function App() {
  return (
    <Card title="User Profile">
      <p>User information goes here</p>
      <button>Edit Profile</button>
    </Card>
  );
}
```

### **Q17: What are React fragments?**
**Answer:** Fragments let you group multiple elements without adding an extra DOM node:
```jsx
function MyComponent() {
  return (
    <React.Fragment>
      <h1>Title</h1>
      <p>Description</p>
    </React.Fragment>
  );
}

// Short syntax
function MyComponent() {
  return (
    <>
      <h1>Title</h1>
      <p>Description</p>
    </>
  );
}
```

### **Q18: How do you style components in React?**
**Answer:**
```jsx
// Inline styles
<div style={{ color: 'red', fontSize: '16px' }}>Text</div>

// CSS classes
<div className="my-component">Text</div>

// CSS Modules
import styles from './Component.module.css';
<div className={styles.container}>Text</div>

// Styled Components
const StyledDiv = styled.div`
  color: red;
  font-size: 16px;
`;
```

### **Q19: What is the difference between state and props?**
**Answer:**
- **Props:** Read-only data passed from parent, immutable
- **State:** Mutable data owned by component, can be updated

### **Q20: How do you update state in React?**
**Answer:**
```jsx
// Functional component
const [count, setCount] = useState(0);
setCount(count + 1);
setCount(prevCount => prevCount + 1); // Functional update

// Class component
this.setState({ count: this.state.count + 1 });
this.setState(prevState => ({ count: prevState.count + 1 }));
```

### **Q21: What is the component lifecycle in React?**
**Answer:** Components go through phases:
- **Mounting:** Component is created and inserted into DOM
- **Updating:** Component is re-rendered due to prop/state changes
- **Unmounting:** Component is removed from DOM

### **Q22: What are the basic lifecycle methods in class components?**
**Answer:**
```jsx
class MyComponent extends React.Component {
  componentDidMount() {
    // After component mounts
  }
  
  componentDidUpdate(prevProps, prevState) {
    // After component updates
  }
  
  componentWillUnmount() {
    // Before component unmounts
  }
  
  render() {
    return <div>Component content</div>;
  }
}
```

### **Q23: How do you prevent a component from rendering?**
**Answer:**
```jsx
function ConditionalComponent({ shouldRender, children }) {
  if (!shouldRender) {
    return null;
  }
  
  return <div>{children}</div>;
}
```

### **Q24: What is the difference between createElement and JSX?**
**Answer:**
```jsx
// JSX
const element = <h1 className="title">Hello</h1>;

// createElement (what JSX compiles to)
const element = React.createElement('h1', { className: 'title' }, 'Hello');
```

### **Q25: How do you handle errors in React components?**
**Answer:**
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    
    return this.props.children;
  }
}
```

---

## 🟡 INTERMEDIATE LEVEL (Questions 26-50)

### **Q26: What is the useContext hook and how do you use it?**
**Answer:**
```jsx
const ThemeContext = React.createContext();

function App() {
  return (
    <ThemeContext.Provider value={{ theme: 'dark' }}>
      <Header />
    </ThemeContext.Provider>
  );
}

function Header() {
  const { theme } = useContext(ThemeContext);
  return <div className={theme}>Header</div>;
}
```

### **Q27: What is the useReducer hook and when would you use it?**
**Answer:**
```jsx
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return initialState;
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

### **Q28: Explain React's reconciliation process.**
**Answer:** React compares the new virtual DOM tree with the previous one and calculates the minimum changes needed to update the real DOM. It uses a diffing algorithm that:
1. Compares elements by type
2. Uses keys to identify list items
3. Updates only changed elements
4. Performs updates in batches

### **Q29: What are React Portals and when would you use them?**
**Answer:**
```jsx
import { createPortal } from 'react-dom';

function Modal({ children, isOpen }) {
  if (!isOpen) return null;
  
  return createPortal(
    <div className="modal-overlay">
      <div className="modal">
        {children}
      </div>
    </div>,
    document.getElementById('modal-root')
  );
}
```

### **Q30: How do you optimize performance in React applications?**
**Answer:**
- **React.memo()** for component memoization
- **useMemo()** for expensive calculations
- **useCallback()** for function memoization
- **Code splitting** with React.lazy()
- **Virtualization** for large lists
- **Proper key usage** in lists

```jsx
const ExpensiveComponent = React.memo(({ data }) => {
  const expensiveValue = useMemo(() => {
    return data.reduce((sum, item) => sum + item.value, 0);
  }, [data]);
  
  return <div>{expensiveValue}</div>;
});
```

### **Q31: What is prop drilling and how can you avoid it?**
**Answer:** Prop drilling is passing props through multiple component levels. Solutions:
- **Context API**
- **State management libraries** (Redux, Zustand)
- **Component composition**

```jsx
// Prop drilling problem
function App() {
  const user = { name: 'John' };
  return <Header user={user} />;
}

function Header({ user }) {
  return <Navigation user={user} />;
}

function Navigation({ user }) {
  return <UserProfile user={user} />;
}

// Solution with Context
const UserContext = createContext();

function App() {
  const user = { name: 'John' };
  return (
    <UserContext.Provider value={user}>
      <Header />
    </UserContext.Provider>
  );
}

function UserProfile() {
  const user = useContext(UserContext);
  return <div>{user.name}</div>;
}
```

### **Q32: How do you implement code splitting in React?**
**Answer:**
```jsx
import { lazy, Suspense } from 'react';

const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
```

### **Q33: What are Higher-Order Components (HOCs)?**
**Answer:**
```jsx
function withLoading(WrappedComponent) {
  return function WithLoadingComponent(props) {
    if (props.isLoading) {
      return <div>Loading...</div>;
    }
    
    return <WrappedComponent {...props} />;
  };
}

const UserListWithLoading = withLoading(UserList);

// Usage
<UserListWithLoading isLoading={loading} users={users} />
```

### **Q34: How do you create custom hooks?**
**Answer:**
```jsx
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });
  
  const setValue = (value) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [storedValue, setValue];
}

// Usage
function App() {
  const [name, setName] = useLocalStorage('name', '');
  
  return (
    <input 
      value={name} 
      onChange={(e) => setName(e.target.value)} 
    />
  );
}
```

### **Q35: How do you handle asynchronous operations in useEffect?**
**Answer:**
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    let isCancelled = false;
    
    async function fetchUser() {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        
        if (!isCancelled) {
          setUser(userData);
        }
      } catch (error) {
        if (!isCancelled) {
          console.error('Error fetching user:', error);
        }
      } finally {
        if (!isCancelled) {
          setLoading(false);
        }
      }
    }
    
    fetchUser();
    
    return () => {
      isCancelled = true;
    };
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  
  return <div>{user?.name}</div>;
}
```

### **Q36: What is the difference between useMemo and useCallback?**
**Answer:**
- **useMemo:** Memoizes the result of a computation
- **useCallback:** Memoizes the function itself

```jsx
function MyComponent({ items, onItemClick }) {
  // useMemo - memoizes the computed value
  const expensiveValue = useMemo(() => {
    return items.filter(item => item.active).length;
  }, [items]);
  
  // useCallback - memoizes the function
  const handleClick = useCallback((id) => {
    onItemClick(id);
  }, [onItemClick]);
  
  return (
    <div>
      Active items: {expensiveValue}
      {items.map(item => (
        <Item key={item.id} onClick={() => handleClick(item.id)} />
      ))}
    </div>
  );
}
```

### **Q37: How do you implement error boundaries in functional components?**
**Answer:** Error boundaries must be class components, but you can use libraries like `react-error-boundary`:
```jsx
import { ErrorBoundary } from 'react-error-boundary';

function ErrorFallback({ error, resetErrorBoundary }) {
  return (
    <div role="alert">
      <h2>Something went wrong:</h2>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}

function App() {
  return (
    <ErrorBoundary
      FallbackComponent={ErrorFallback}
      onError={(error, errorInfo) => console.error(error, errorInfo)}
    >
      <MyComponent />
    </ErrorBoundary>
  );
}
```

### **Q38: How do you test React components?**
**Answer:**
```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments counter when button is clicked', () => {
  render(<Counter />);
  
  const button = screen.getByText('Increment');
  const counter = screen.getByText(/count: 0/i);
  
  fireEvent.click(button);
  
  expect(screen.getByText(/count: 1/i)).toBeInTheDocument();
});

// Testing hooks
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

test('should increment counter', () => {
  const { result } = renderHook(() => useCounter());
  
  act(() => {
    result.current.increment();
  });
  
  expect(result.current.count).toBe(1);
});
```

### **Q39: What are render props?**
**Answer:**
```jsx
function MouseTracker({ render }) {
  const [mouse, setMouse] = useState({ x: 0, y: 0 });
  
  const handleMouseMove = (event) => {
    setMouse({
      x: event.clientX,
      y: event.clientY
    });
  };
  
  return (
    <div onMouseMove={handleMouseMove}>
      {render(mouse)}
    </div>
  );
}

// Usage
function App() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <h1>The mouse position is ({x}, {y})</h1>
      )}
    />
  );
}
```

### **Q40: How do you handle focus management in React?**
**Answer:**
```jsx
function FocusableList({ items }) {
  const [focusedIndex, setFocusedIndex] = useState(0);
  const itemRefs = useRef([]);
  
  useEffect(() => {
    itemRefs.current[focusedIndex]?.focus();
  }, [focusedIndex]);
  
  const handleKeyDown = (e) => {
    switch (e.key) {
      case 'ArrowDown':
        e.preventDefault();
        setFocusedIndex(prev => 
          Math.min(prev + 1, items.length - 1)
        );
        break;
      case 'ArrowUp':
        e.preventDefault();
        setFocusedIndex(prev => Math.max(prev - 1, 0));
        break;
    }
  };
  
  return (
    <ul onKeyDown={handleKeyDown}>
      {items.map((item, index) => (
        <li
          key={item.id}
          ref={el => itemRefs.current[index] = el}
          tabIndex={index === focusedIndex ? 0 : -1}
        >
          {item.name}
        </li>
      ))}
    </ul>
  );
}
```

### **Q41: What is React.StrictMode?**
**Answer:** StrictMode helps identify problems in development:
```jsx
function App() {
  return (
    <React.StrictMode>
      <MyComponent />
    </React.StrictMode>
  );
}
```
It:
- Identifies unsafe lifecycles
- Warns about deprecated APIs
- Detects unexpected side effects
- Ensures reusable state

### **Q42: How do you implement drag and drop in React?**
**Answer:**
```jsx
function DragDropExample() {
  const [draggedItem, setDraggedItem] = useState(null);
  
  const handleDragStart = (e, item) => {
    setDraggedItem(item);
    e.dataTransfer.effectAllowed = 'move';
  };
  
  const handleDragOver = (e) => {
    e.preventDefault();
    e.dataTransfer.dropEffect = 'move';
  };
  
  const handleDrop = (e, targetZone) => {
    e.preventDefault();
    if (draggedItem) {
      // Handle drop logic
      console.log(`Dropped ${draggedItem.name} into ${targetZone}`);
      setDraggedItem(null);
    }
  };
  
  return (
    <div>
      <div
        draggable
        onDragStart={(e) => handleDragStart(e, { name: 'Item 1' })}
      >
        Drag me
      </div>
      
      <div
        onDragOver={handleDragOver}
        onDrop={(e) => handleDrop(e, 'target-zone')}
        className="drop-zone"
      >
        Drop here
      </div>
    </div>
  );
}
```

### **Q43: How do you implement infinite scrolling in React?**
**Answer:**
```jsx
function InfiniteScroll() {
  const [items, setItems] = useState([]);
  const [loading, setLoading] = useState(false);
  const [hasMore, setHasMore] = useState(true);
  const observer = useRef();
  
  const lastItemRef = useCallback(node => {
    if (loading) return;
    if (observer.current) observer.current.disconnect();
    
    observer.current = new IntersectionObserver(entries => {
      if (entries[0].isIntersecting && hasMore) {
        loadMoreItems();
      }
    });
    
    if (node) observer.current.observe(node);
  }, [loading, hasMore]);
  
  const loadMoreItems = async () => {
    setLoading(true);
    try {
      const newItems = await fetchItems(items.length);
      setItems(prev => [...prev, ...newItems]);
      setHasMore(newItems.length > 0);
    } catch (error) {
      console.error('Error loading items:', error);
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <div>
      {items.map((item, index) => (
        <div
          key={item.id}
          ref={index === items.length - 1 ? lastItemRef : null}
        >
          {item.content}
        </div>
      ))}
      {loading && <div>Loading...</div>}
    </div>
  );
}
```

### **Q44: How do you implement a search with debouncing?**
**Answer:**
```jsx
function SearchWithDebounce() {
  const [searchTerm, setSearchTerm] = useState('');
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);
  
  const debouncedSearchTerm = useDebounce(searchTerm, 500);
  
  useEffect(() => {
    if (debouncedSearchTerm) {
      searchItems(debouncedSearchTerm);
    } else {
      setResults([]);
    }
  }, [debouncedSearchTerm]);
  
  const searchItems = async (term) => {
    setLoading(true);
    try {
      const response = await fetch(`/api/search?q=${term}`);
      const data = await response.json();
      setResults(data);
    } catch (error) {
      console.error('Search error:', error);
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      {loading && <div>Searching...</div>}
      <ul>
        {results.map(result => (
          <li key={result.id}>{result.title}</li>
        ))}
      </ul>
    </div>
  );
}

function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]);
  
  return debouncedValue;
}
```

### **Q45: How do you handle form validation in React?**
**Answer:**
```jsx
function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});
  
  const validateField = (name, value) => {
    switch (name) {
      case 'name':
        return value.length < 2 ? 'Name must be at least 2 characters' : '';
      case 'email':
        return !/\S+@\S+\.\S+/.test(value) ? 'Email is invalid' : '';
      case 'message':
        return value.length < 10 ? 'Message must be at least 10 characters' : '';
      default:
        return '';
    }
  };
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    
    if (touched[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: validateField(name, value)
      }));
    }
  };
  
  const handleBlur = (e) => {
    const { name, value } = e.target;
    setTouched(prev => ({ ...prev, [name]: true }));
    setErrors(prev => ({
      ...prev,
      [name]: validateField(name, value)
    }));
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    const newErrors = {};
    Object.keys(formData).forEach(key => {
      newErrors[key] = validateField(key, formData[key]);
    });
    
    setErrors(newErrors);
    setTouched(Object.keys(formData).reduce((acc, key) => ({
      ...acc,
      [key]: true
    }), {}));
    
    const hasErrors = Object.values(newErrors).some(error => error);
    if (!hasErrors) {
      console.log('Form submitted:', formData);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          name="name"
          value={formData.name}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Name"
        />
        {touched.name && errors.name && (
          <span className="error">{errors.name}</span>
        )}
      </div>
      
      <div>
        <input
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Email"
        />
        {touched.email && errors.email && (
          <span className="error">{errors.email}</span>
        )}
      </div>
      
      <div>
        <textarea
          name="message"
          value={formData.message}
          onChange={handleChange}
          onBlur={handleBlur}
          placeholder="Message"
        />
        {touched.message && errors.message && (
          <span className="error">{errors.message}</span>
        )}
      </div>
      
      <button type="submit">Submit</button>
    </form>
  );
}
```

### **Q46: What is React Suspense and how do you use it?**
**Answer:**
```jsx
import { Suspense, lazy } from 'react';

const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading component...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

// For data fetching (experimental)
function ProfilePage({ userId }) {
  return (
    <Suspense fallback={<h1>Loading profile...</h1>}>
      <ProfileDetails userId={userId} />
      <Suspense fallback={<h1>Loading posts...</h1>}>
        <ProfileTimeline userId={userId} />
      </Suspense>
    </Suspense>
  );
}
```
