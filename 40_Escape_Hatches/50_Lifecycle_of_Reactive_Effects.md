### Жизненный цикл компонента

- mount
- updates
- unmounts

Effect синхронизирует внешнюю систему с props and state.

---

### Пример синхронизации

```typescript
const serverUrl = 'https://localhost:1234';
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId]);
  // ...
}
```
---

### Пример повторной синхронизации

```typescript
function ChatRoom({ roomId /* "general" */ }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect(); 
    };
  }, [roomId]);
```
```typescript
function ChatRoom({ roomId /* "travel" */ }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect(); 
    };
  }, [roomId]);
```

---

### Жизненный цикл effect

- Your Effect connected to the "general" room
- Your Effect disconnected from the "general" room and connected to the "travel" room
- Your Effect disconnected from the "travel" room and connected to the "music" room
- Your Effect disconnected from the "music" room

---

### Как react проверяет, что effect может пересинхронизироваться

```4/5/App1```

---

### Откуда react знает, что нужно пересинхронизировать effect

```4/5/App1```

---

### Каждый effect - отдельный процесс синхронизации

```typescript
function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId);
  }, [roomId]);

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    // ...
  }, [roomId]);
  // ...
}
```

---

### Effect "реагирует" только на реактивные переменные

```typescript
const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId]);
  // ...
}
```

---

### Все переменные в теле компонента - реактивные

```typescript
function ChatRoom({ roomId, selectedServerUrl }) { 
  // roomId is reactive
  const settings = useContext(SettingsContext); 
  // settings is reactive
  const serverUrl = 
    selectedServerUrl ?? settings.defaultServerUrl;
 // serverUrl is reactive
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); 
    // Your Effect reads roomId and serverUrl
    connection.connect();
    return () => {
      connection.disconnect();
    }; }, [roomId, serverUrl]); }
```

---

### Как работает effect  с пустым списком зависимостей

```typescript
const serverUrl = 'https://localhost:1234';
const roomId = 'general';

function ChatRoom() {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, []); // ✅ All dependencies declared
  // ...
}
```

---

### React проверяет список зависимостей

---

### Как удалить переменную из списка зависимостей

- Проверьте, что effect - независимый процесс синхронизации
- Разделите переменные на те, на которые нужно реагировать, и не те, на которые не нужно. Вторые можно поместить в Effect Event (см. далее).
- Остерегайтесь использовать объекты и функции в качестве зависимости эффекта (см. далее).

---

### Резюме

<small><ul>
<li>Компонент может mount, updates и unmount</li>
<li>Каждый effect имеет отдельный от компонента жизненный цикл.</li>
<li>Каждый effect описывает отдельный процесс синхронизации, который может быть запущен и остановлен.</li>
<li>При написании effect отталкивайтесь от жизненного цикла effect, а не компонента.</li>
<li>Переменные в теле компонента - реактивные.</li>
<li>Реактивные переменные должны пересинхронизировать effect при изменении.</li>
<li>linter проверяет список зависимостей.</li>
<li>Не нужно игнорировать предупреждения linter.</li>
<ul><small>