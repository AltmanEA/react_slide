### Обработчик событий vs effect

Почему код должен быть выполнен?

- Обработчик событий запускается в ответ на действия пользователя. Например: отправка сообщений пользователя.
- Effect запускается не зависимо от действий пользователя. Например: присоединение к серверу чата.

---

### Обработчик событий vs effect

Реакций на изменение props, state и другие реактивные переменные.

- Обработчики событий не реагирую на изменение реактивных переменных.
- Effect реагирует на изменение реактивных переменных, от которых он зависит.

---

### Реактивная логика в effect

```typescript
function ChatRoom({ roomId, theme }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      showNotification('Connected!', theme);
    });
    connection.connect();
    // ...
```

---

### Извлечение реактивной логики из effect

```typescript
function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => {
    showNotification('Connected!', theme);
  });   // Экспериментальная версия !!!
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      onConnected();
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // ✅ All dependencies declared
  // ...
```

---

### Использование реактивных свойств в effect, проблема

```typescript
function Page({ url }) {
  const { items } = useContext(ShoppingCartContext);
  const numberOfItems = items.length;

  useEffect(() => {
    logVisit(url, numberOfItems);
  }, [url]); 
  // 🔴 React Hook useEffect 
  // has a missing dependency: 'numberOfItems'
  // ...
}
```

---

### Использование реактивных свойств в effect, решение

```typescript
function Page({ url }) {
  const { items } = useContext(ShoppingCartContext);
  const numberOfItems = items.length;
    const onVisit = useEffectEvent(visitedUrl => {
    logVisit(visitedUrl, numberOfItems);
  });
  useEffect(() => {
    onVisit(url);
  }, [url]); // ✅ All dependencies declared
  // ...
}
```

---

### Ограничения effect event

- Может вызываться только в effect;
- Не может передаваться в другие компоненты или хуки.

---

### Пока effect event экспериментальные

```typescript
function Page({ url }) {
  const { items } = useContext(ShoppingCartContext);
  const numberOfItems = items.length;
  useEffect(() => {
    logVisit(url, numberOfItems);
    // 🔴 Avoid suppressing the linter like this:
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [url]);
  // ...
}
```


---

### Резюме

<small><ul>
<li>Обработчик событий запускается в ответ на действия пользователя.</li>
<li>Effect запускается при необходимости синхронизации.</li>
<li>Логика внутри обработчика событий не реактивна.</li>
<li>Логика внутри effect реактивна.</li>
<li>Не реактивную логику из effect можно перенести в effect event.</li>
<li>Effect event можно вызывать только из effect.</li>
<li>Не передавайте effect event другим компонентам или хукам.</li>
<ul><small>