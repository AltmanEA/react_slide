### Случаи, когда не  нужно применять effect

- Преобразование данных для render.
- Обработка действий пользователя.

---

### Пример: обновление state на основе props и state

```typescript
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  // 🔴 Avoid: redundant state and unnecessary Effect
  const [fullName, setFullName] = useState('');
  useEffect(() => {
    setFullName(firstName + ' ' + lastName);
  }, [firstName, lastName]);
  // ...
}
```
```typescript
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  // ✅ Good: calculated during rendering
  const fullName = firstName + ' ' + lastName;
  // ...
}
```

---

### Пример: кэширование результатов вычислений

```typescript
function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  // 🔴 Avoid: redundant state and unnecessary Effect
  const [visibleTodos, setVisibleTodos] = useState([]);
  useEffect(() => {
    setVisibleTodos(getFilteredTodos(todos, filter));
  }, [todos, filter]);
  // ...
}
```
```typescript
import { useMemo, useState } from 'react';

function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  const visibleTodos = useMemo(() => {
    // ✅ Does not re-run unless todos or filter change
    return getFilteredTodos(todos, filter);
  }, [todos, filter]);
  // ...
}
```

---

### Пример: сброс state при изменении props

```typescript
export default function ProfilePage({ userId }) {
  const [comment, setComment] = useState('');
  // 🔴 Avoid: Resetting state on prop change in an Effect
  useEffect(() => {
    setComment('');
  }, [userId]);
  // ...
}
```
```typescript
export default function ProfilePage({ userId }) {
  return (
    <Profile userId={userId} key={userId} />
  );
}
function Profile({ userId }) {
  // ✅ This and any other state below will reset 
  //  on key change automatically
  const [comment, setComment] = useState('');
  // ...
}
```

---

### Пример: Подстройка state при изменении props

```typescript
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);
  // 🔴 Avoid: Adjusting state on prop change in an Effect
  useEffect(() => {
    setSelection(null);
  }, [items]); // ... 
}
```
```typescript
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);
  // Better: Adjust the state while rendering
  const [prevItems, setPrevItems] = useState(items);
  if (items !== prevItems) {
    setPrevItems(items);
    setSelection(null);
  } // ... 
}
```

---

### Пример: Подстройка state при изменении props

```typescript
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);
  // Better: Adjust the state while rendering
  const [prevItems, setPrevItems] = useState(items);
  if (items !== prevItems) {
    setPrevItems(items);
    setSelection(null);
  } // ... 
}
```
```typescript
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selectedId, setSelectedId] = useState(null);
  // ✅ Best: Calculate everything during rendering
  const selection = items
    .find(item => item.id === selectedId) ?? null; // ... 
}
```

---

### Пример: Общая логика обработчиков событий

```typescript
function ProductPage({ product, addToCart }) {
  // 🔴 Avoid: Event-specific logic inside an Effect
  useEffect(() => {
    if (product.isInCart) {
      showNotification(`Added ${product.name}!`);
    } }, [product]);
  function handleBuyClick() {
    addToCart(product);
  }
  function handleCheckoutClick() {
    addToCart(product);
    navigateTo('/checkout');
  } // ...
}
```

---

### Пример: Общая логика обработчиков событий

```typescript
function ProductPage({ product, addToCart }) {
  // ✅ Good: Event-specific logic is called from event handlers
  function buyProduct() {
    addToCart(product);
    showNotification(`Added ${product.name}!`);
  }
  function handleBuyClick() {
    buyProduct();
  }
  function handleCheckoutClick() {
    buyProduct();
    navigateTo('/checkout');
  } // ...
}
```

---

### Пример: POST запрос

```typescript
function Form() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');
  // ✅ Good: This logic should run because 
  //   the component was displayed
  useEffect(() => {
    post('/analytics/event', { eventName: 'visit_form' });
  }, []);
  // 🔴 Avoid: Event-specific logic inside an Effect
  const [jsonToSubmit, setJsonToSubmit] = useState(null);
  useEffect(() => { if (jsonToSubmit !== null) { 
      post('/api/register', jsonToSubmit);
    } }, [jsonToSubmit]);
  function handleSubmit(e) { ... } // ...
}
```

---

### Пример: POST запрос

```typescript
function Form() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');
  // ✅ Good: This logic should run because 
  //   the component was displayed
  useEffect(() => {
    post('/analytics/event', { eventName: 'visit_form' });
  }, []);
  function handleSubmit(e) {
    e.preventDefault();
    // ✅ Good: Event-specific logic is in the event handler
    post('/api/register', { firstName, lastName });
  } // ...
}
```

---

### Пример: цепочка вычислений

```typescript
function Game() {
  const [card, setCard] = useState(null);
  const [goldCardCount, setGoldCardCount] = useState(0);
  const [round, setRound] = useState(1);
  const [isGameOver, setIsGameOver] = useState(false);
  // 🔴 Avoid: Chains of Effects that adjust 
  //  the state solely to trigger each other
  useEffect(() => { if (card !== null && card.gold) {
      setGoldCardCount(c => c + 1); } }, [card]);
  useEffect(() => { if (goldCardCount > 3) {
      setRound(r => r + 1)
      setGoldCardCount(0); } }, [goldCardCount]);
  useEffect(() => { if (round > 5) {
      setIsGameOver(true); } }, [round]);
  useEffect(() => { alert('Good game!'); }, [isGameOver]);
  // ...
```

---

### Пример: цепочка вычислений

```typescript
function Game() {
  const [card, setCard] = useState(null);
  const [goldCardCount, setGoldCardCount] = useState(0);
  const [round, setRound] = useState(1);
  // ✅ Calculate what you can during rendering
  const isGameOver = round > 5;
  function handlePlaceCard(nextCard) {
    if (isGameOver) { throw Error('Game already ended.'); }
    // ✅ Calculate all the next state in the event handler
    setCard(nextCard);
    if (nextCard.gold) {
      if (goldCardCount <= 3) {
        setGoldCardCount(goldCardCount + 1);
      } else {
        setGoldCardCount(0);
        setRound(round + 1);
        if (round === 5) {
          alert('Good game!');
        }
      }
    }
  }  // ...
```

---

### Пример: Инициализация приложения

```typescript
function App() {
  // 🔴 Avoid: Effects with logic that should only ever run once
  useEffect(() => {
    loadDataFromLocalStorage();
    checkAuthToken();
  }, []); // ...
}
```
```typescript
// Check if we're running in the browser.
if (typeof window !== 'undefined') { 
   // ✅ Only runs once per app load
  checkAuthToken();
  loadDataFromLocalStorage();
}
function App() {
  // ...
}
```

---

### Пример: Уведомление родителей о изменении state

```typescript
function Toggle({ onChange }) {
  const [isOn, setIsOn] = useState(false);
  // 🔴 Avoid: The onChange handler runs too late
  useEffect(() => {
    onChange(isOn);
  }, [isOn, onChange])
  function handleClick() {
    setIsOn(!isOn);
  }
  function handleDragEnd(e) {
    if (isCloserToRightEdge(e)) {
      setIsOn(true);
    } else {
      setIsOn(false);
    }
  }// ...
}
```

---

### Пример: Уведомление родителей о изменении state

```typescript
function Toggle({ onChange }) {
  const [isOn, setIsOn] = useState(false);
  function updateToggle(nextIsOn) {
    // ✅ Good: Perform all updates during the event 
    //  that caused them
    setIsOn(nextIsOn);
    onChange(nextIsOn);
  }
  function handleClick() {
    updateToggle(!isOn);
  }
  function handleDragEnd(e) {
    if (isCloserToRightEdge(e)) {
      updateToggle(true);
    } else {
      updateToggle(false);
    }
  }
  // ...
}
```

---

### Пример: Уведомление родителей о изменении state

```typescript
// ✅ Also good: the component is fully controlled 
//  by its parent
function Toggle({ isOn, onChange }) {
  function handleClick() {
    onChange(!isOn);
  }
  function handleDragEnd(e) {
    if (isCloserToRightEdge(e)) {
      onChange(true);
    } else {
      onChange(false);
    }
  }
  // ...
}
```

---

### Пример: Передача данных родителям

```typescript
function Parent() {
  const [data, setData] = useState(null);
  // ...
  return <Child onFetched={setData} />;
}
function Child({ onFetched }) {
  const data = useSomeAPI();
  // 🔴 Avoid: Passing data to the parent in an Effect
  useEffect(() => {
    if (data) {
      onFetched(data);
    }
  }, [onFetched, data]); // ...
}
```

---

### Пример: Передача данных родителям

```typescript
function Parent() {
  const data = useSomeAPI();
  // ...
  // ✅ Good: Passing data down to the child
  return <Child data={data} />;
}
function Child({ data }) {
  // ...
}
```

---

### Пример: Подписка на внешнее хранилище

```typescript
function useOnlineStatus() {
  // Not ideal: Manual store subscription in an Effect
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function updateState() {
      setIsOnline(navigator.onLine);
    }
    updateState();
    window.addEventListener('online', updateState);
    window.addEventListener('offline', updateState);
    return () => {
      window.removeEventListener('online', updateState);
      window.removeEventListener('offline', updateState);
    };
  }, []);
  return isOnline;
}
function ChatIndicator() {
  const isOnline = useOnlineStatus();
  // ...
}
```

---

### Пример: Подписка на внешнее хранилище

```typescript
function useOnlineStatus() {
  // ✅ Good: Subscribing to an external store 
  //  with a built-in Hook
  return useSyncExternalStore(
    subscribe, // React won't resubscribe for 
            //as long as you pass the same function
    // How to get the value on the client        
    () => navigator.onLine,
    // How to get the value on the server 
    () => true 
  );
}
function ChatIndicator() {
  const isOnline = useOnlineStatus();
  // ...
}
function subscribe(callback) {
  window.addEventListener('online', callback);
  window.addEventListener('offline', callback);
  return () => {
    window.removeEventListener('online', callback);
    window.removeEventListener('offline', callback);
  }; }
```

---

### Пример: Запрос данных с сервера

```typescript
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  const [page, setPage] = useState(1);
  useEffect(() => {
    // 🔴 Avoid: Fetching without cleanup logic
    fetchResults(query, page).then(json => {
      setResults(json);
    });
  }, [query, page]);
  function handleNextPageClick() {
    setPage(page + 1);
  }
  // ...
}
```

---

### Пример: Запрос данных с сервера

```typescript
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  const [page, setPage] = useState(1);
  useEffect(() => {
    let ignore = false;
    fetchResults(query, page).then(json => {
      if (!ignore) {
        setResults(json);
      }
    });
    return () => {
      ignore = true;
    };
  }, [query, page]);
  function handleNextPageClick() {
    setPage(page + 1);
  }
  // ...
}
```


---

### Пример: Запрос данных с сервера

```typescript
function useData(url) {
  const [data, setData] = useState(null);
  useEffect(() => {
    let ignore = false;
    fetch(url)
      .then(response => response.json())
      .then(json => {if (!ignore) {
          setData(json); }
      });
    return () => { ignore = true; };
  }, [url]);
  return data;
}
```