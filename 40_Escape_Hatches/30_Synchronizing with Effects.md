### Синхронизация с внешним миром

- запросы к серверу
- отправка данных
- сообщения пользователю

---

### Виды логики в react

- Render
- Обработчики событий
- Effect

Пример: соединение с сервером в чате.

---

### Как писать эффект

- Объявить эффект (по умолчанию вызывается после каждого commit).
- Определить зависимости эффекта.
- Определить функцию очистики.

---

### Объявление эффекта

```typescript
import { useEffect } from 'react';

...
function MyComponent() {
  useEffect(() => {
    // Code here will run after *every* render
  });
  return <div />;
}
```

---

### Пример: проблема синхронизации видеоплеера

```4/3/App1```

---

### Пример: решение проблемы синхронизации видеоплеера

```4/3/App2```

---

### Пример: не нужные вызовы эффектов

```4/3/App3```

---

### Пример: убираем ненужные вызовы с помощью зависимостей

```4/3/App4```

```4/3/App5```

---

### Пример: проблема очистки

```4/3/App6```

---

### Пример: решение проблемы очистки

```4/3/App7```

---

### Про двойной рендеринг в режиме отладки

---

### Использование effect: контроль виджетов

```typescript
useEffect(() => {
  const map = mapRef.current;
  map.setZoomLevel(zoomLevel);
}, [zoomLevel]);
```
```typescript
useEffect(() => {
  const dialog = dialogRef.current;
  dialog.showModal();
  return () => dialog.close();
}, []);
```

---

### Использование effect: подписка на события

```typescript
useEffect(() => {
  function handleScroll(e) {
    console.log(window.scrollX, window.scrollY);
  }
  window.addEventListener('scroll', handleScroll);
  return () => window.removeEventListener('scroll', handleScroll);
}, []);
```

---

### Использование effect: запуск анимации

```typescript
useEffect(() => {
  const node = ref.current;
  node.style.opacity = 1; // Trigger the animation
  return () => {
    node.style.opacity = 0; // Reset to the initial value
  };
}, []);
```

---

### Использование effect: выборка данных

```typescript
useEffect(() => {
  let ignore = false;
  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }
  startFetching();
  return () => {
    ignore = true;
  };
}, [userId]);
```

---

### Использование effect: отправка логов

```typescript
useEffect(() => {
  logVisit(url); // Sends a POST request
}, [url]);
```

---

### Решение без effect: инициализация приложения

```typescript
if (typeof window !== 'undefined') { 
  // Check if we're running in the browser.
  checkAuthToken();
  loadDataFromLocalStorage();
}
function App() {
  // ...
}
```

---

### Решение без effect: покупка продуктов

```typescript
function handleClick() {
  // ✅ Buying is an event because 
  // it is caused by a particular interaction.
  fetch('/api/buy', { method: 'POST' });
}
```

---

### Большой пример с effect

```4/3/App9```

---

### Резюме

<small><ul>
<li>В отличие от events, effects вызываются render, а не действиями пользователей</li>
<li>Effects позволяют синхронизировать компоненты с внешними системами (браузер, сеть, другие приложения).</code>.</li>
<li>По-умолчанию effect запускается после каждого render.</li>
<li>React не перезапустить effect если все его зависимости не изменились.</li>
<li>Вы не можете выбирать зависимости effect</li>
<li>При пустом списке зависимостей effect будет вызываться только при создании компонента</li>
<li>В режиме отладке и Strict Mode effects будут вызываться дважды для проверки корректности их работы.</li>
<li>Если effect не работает из-за удаления компонента нужно реализовать функцию очистки.</li>
<li>React вызывает функцию очистки перед повторным запуском эффекта и при удалении компонента</li>
<ul><small>