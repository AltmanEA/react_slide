### Общая логика: статус сетевого подключения

```typescript
export default function StatusBar() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function handleOnline() { setIsOnline(true); }
    function handleOffline() { setIsOnline(false); }
    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);
  return <h1>{isOnline ? '✅ Online' : '❌ Disconnected'}</h1>;
}
```

---

### Хук для статуса сетевого подключения

```typescript
function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function handleOnline() { setIsOnline(true); }
    function handleOffline() { setIsOnline(false); }
    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);
  return isOnline;
}
```

---

### Соглашения в react

- Имена компонентов записываются с заглавной буквы (```StatusBar```).
- Имена хуков начинаются в ```use``` (```useOnlineStatus```).

---

### Пример: хук для поля ввода

```typescript
export function useFormInput(initialValue) {
  const [value, setValue] = useState(initialValue);
  function handleChange(e) {
    setValue(e.target.value);
  }
  const inputProps = {
    value: value,
    onChange: handleChange
  };
  return inputProps;
}
```

---

### Хуки не разделяют состояние

```typescript
export default function Form() {
    const firstNameProps = useFormInput('Mary');
    const lastNameProps = useFormInput('Poppins');
    return (<>
      <label> First name:
        <input {...firstNameProps} />
      </label>
      <label> Last name:
        <input {...lastNameProps} />
      </label>
      <p><b>Good morning, 
      {firstNameProps.value} {lastNameProps.value}.</b></p>
    </>);
}
```

---

### Хук и реактивные переменные

```typescript
export function useChatRoom({ serverUrl, roomId }) {
  useEffect(() => {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId
    };
    const connection = createConnection(options);
    connection.connect();
    connection.on('message', (msg) => {
      showNotification('New message: ' + msg);
    });
    return () => connection.disconnect();
  }, [roomId, serverUrl]);
}
```

---

### Передача хукам обработчика событий

```typescript
export function useChatRoom(
    { serverUrl, roomId, onReceiveMessage }) {
  useEffect(() => {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId };
    const connection = createConnection(options);
    connection.connect();
    connection.on('message', (msg) = > {
      onReceiveMessage(msg);
    });
    return () => connection.disconnect();
  }, [roomId, serverUrl, onReceiveMessage]); 
  // ✅ All dependencies declared }
```

---

### Передача хукам обработчика событий

```typescript
export function useChatRoom(
    { serverUrl, roomId, onReceiveMessage }) {
    const onMessage = useEffectEvent(onReceiveMessage);
    useEffect(() => {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId };
    const connection = createConnection(options);
    connection.connect();
    connection.on('message', (msg) => {
      onMessage(msg); });
    return () => connection.disconnect();
  }, [roomId, serverUrl]); 
  // ✅ All dependencies declared }
```

---

### Когда и зачем нужно писать хук

- Для устранения небольшого дублирования кода (например, ```useFormInput```) писать хук - излишне.
- Отделить логику взаимодействия с внешними системами - полезно.
- Инкапсулирование effect в хук позволяет не переписывать компоненты при появлении новых возможностей react.

---

### Пример инкапсулирования effect

```typescript
export function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function handleOnline() { setIsOnline(true); }
    function handleOffline() { setIsOnline(false); }
    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);
  return isOnline; }
```

---

### Пример инкапсулирования effect, react 18

```typescript
function subscribe(callback) {
  window.addEventListener('online', callback);
  window.addEventListener('offline', callback);
  return () => {
    window.removeEventListener('online', callback);
    window.removeEventListener('offline', callback);
  }; }
export function useOnlineStatus() {
  return useSyncExternalStore(
    subscribe,
    () => navigator.onLine, // How to get the value on the client
    () => true // How to get the value on the server
  );
}
```