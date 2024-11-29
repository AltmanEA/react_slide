### Проверка списка зависимостей

```typescript
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, []); 
  // 🔴 React Hook useEffect has a missing dependency: 'roomId'
  // ...
}
```

---

### Корректный список зависимостей

```typescript
const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) { // This is a reactive value
  useEffect(() => {
    // This Effect reads that reactive value
    const connection = createConnection(serverUrl, roomId); 
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); 
  // ✅ So you must specify that reactive value 
  //  as a dependency of your Effect
  // ...
}
```

---

### Удаление элементов из списка зависимостей

```typescript
const serverUrl = 'https://localhost:1234';
const roomId = 'music'; // Not a reactive value anymore

function ChatRoom() {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, []); // ✅ All dependencies declared
  // ...
}
```

---

### Алгоритм решения проблемы зависимостями

1. изменить код effect или способ определения реактивных переменных
2. использую подсказки linter обновить список зависимостей
3. если не устраивает список зависимостей вернуться к п.1. 

---

### Причины лишних зависимостей

- различные части effect выполняются при различных условиях
- нужно считать только последнее значение, а не реагировать на изменение реактивных переменных
- зависимость изменяется часто непреднамеренно, если она является объектом или функцией

---

### Решение: перенести код в обработчик событий

```typescript
function Form() {
  const [submitted, setSubmitted] = useState(false);
  useEffect(() => {
    if (submitted) {
      // 🔴 Avoid: Event-specific logic inside an Effect
      post('/api/register');
      showNotification('Successfully registered!');
    }
  }, [submitted]);
  function handleSubmit() {
    setSubmitted(true);
  }
  // ...
}
```

---

### Решение: перенести код в обработчик событий

```typescript
function Form() {
  const [submitted, setSubmitted] = useState(false);
  const theme = useContext(ThemeContext);
  useEffect(() => {
    if (submitted) {
      // 🔴 Avoid: Event-specific logic inside an Effect
      post('/api/register');
      showNotification('Successfully registered!', theme);
    }
  }, [submitted, theme]); // ✅ All dependencies declared
  function handleSubmit() {
    setSubmitted(true);
  }  // ...
}
```

---

### Решение: разделить на несколько эффектов

```typescript
function ShippingForm({ country }) { ...
useEffect(() => {
    let ignore = false;
    fetch(`/api/cities?country=${country}`)
      .then(response => response.json())
      .then(json => {if (!ignore) { setCities(json); } });
//🔴Avoid: A single Effect synchronizes two independent processes
    if (city) { fetch(`/api/areas?city=${city}`)
        .then(response => response.json())
        .then(json => { if (!ignore) {
            setAreas(json); } }); }
    return () => { ignore = true; };
  }, [country, city]); // ✅ All dependencies declared
  // ...
```

---

### Решение: использование функции обновления

```typescript
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    connection.on('message', (receivedMessage) => {
      setMessages([...messages, receivedMessage]);
    });
    // ...
```

---

### Решение: использование функции обновления

```typescript
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    connection.on('message', (receivedMessage) => {
      setMessages(msgs => [...msgs, receivedMessage]);
    });
    return () => connection.disconnect();
  }, [roomId]); // ✅ All dependencies declared
  // ...
```

---

### Решение: считывание текущего значения

```typescript
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);
  const [isMuted, setIsMuted] = useState(false);

  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    connection.on('message', (receivedMessage) => {
      setMessages(msgs => [...msgs, receivedMessage]);
      if (!isMuted) {
        playSound();
      }
    });
    // ...
```

---

### Решение: считывание текущего значения

```typescript
  const onMessage = useEffectEvent(receivedMessage => {
    setMessages(msgs => [...msgs, receivedMessage]);
    if (!isMuted) {
      playSound();
    }
  });
```

---

### Решение: вызов обработчика событий в effect

```typescript
function ChatRoom({ roomId, onReceiveMessage }) {
  const [messages, setMessages] = useState([]);

  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    connection.on('message', (receivedMessage) => {
      onReceiveMessage(receivedMessage);
    });
    return () => connection.disconnect();
  }, [roomId, onReceiveMessage]); // ✅ All dependencies declared
  // ...
```

---

### Решение: вызов обработчика событий в effect

```typescript
const onMessage = useEffectEvent(receivedMessage => {
    onReceiveMessage(receivedMessage);
});

useEffect(() => {
    const connection = createConnection();
    connection.connect();
    connection.on('message', (receivedMessage) => {
      onMessage(receivedMessage);
    });
    return () => connection.disconnect();
 }, [roomId]); // ✅ All dependencies declared
```

---

### Решение: ненамеренное считывание значений

```typescript
function ChatRoom({ roomId }) {
  // ...
  const options = {
    serverUrl: serverUrl,
    roomId: roomId
  };

useEffect(() => {
    const connection = createConnection(options);
    connection.connect();
    // ...
```

---

### Решение: ненамеренное считывание значений

```typescript
const options = {
  serverUrl: 'https://localhost:1234',
  roomId: 'music'
};
function ChatRoom() {
  const [message, setMessage] = useState('');
  useEffect(() => {
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, []); // ✅ All dependencies declared
  // ...
```

---

### Решение: ненамеренное считывание значений

```typescript

function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');

  useEffect(() => {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId
    };
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // ✅ All dependencies declared
  // ...
```

---

### Решение: считывание свойств объекта

```typescript
function ChatRoom({ options }) {
  const [message, setMessage] = useState('');

  useEffect(() => {
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [options]); // ✅ All dependencies declared
  // ...
```

---

### Решение: считывание свойств объекта

```typescript
function ChatRoom({ options }) {
  const [message, setMessage] = useState('');

  const { roomId, serverUrl } = options;
  useEffect(() => {
    const connection = createConnection({
      roomId: roomId,
      serverUrl: serverUrl
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, serverUrl]); // ✅ All dependencies declared
  // ...
```
