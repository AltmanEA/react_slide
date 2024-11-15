### Ref

Используется для хранения "внешних" переменных, изменение которых не вызывает новый render.

---

### Пример Ref

```4/1/App1```

```typescript
import { useRef } from 'react';

let ref = useRef(0);

ref.current = ref.current + 1;
```

---

### Пример ref и state

```4/1/App3```

---

### Различие ref и state

<table>
<tr><th>ref</th><th>state</th>
<tr>
    <td>Возвращает объект с значением внутри</td>
    <td>Возвращает значение и установщик значения</td>
</tr>
</table>

---

### Различие ref и state

<table>
<tr><th>ref</th><th>state</th>
<tr>
    <td>Не вызывает re-render при изменении</td>
    <td>Инициирует re-render</td>
</tr>
</table>

---

### Различие ref и state

<table>
<tr><th>ref</th><th>state</th>
<tr>
    <td>Можно изменять значение</td>
    <td>Нельзя изменять значение</td>
</tr>
</table>

---

### Различие ref и state

<table>
<tr><th>ref</th><th>state</th>
<tr>
    <td>Нельзя читать во время рендера</td>
    <td>Можно читать в любое время</td>
</tr>
</table>

```4/1/App5```

---

### Когда используется ref

- Хранение id таймеров
- Хранение и изменение элементов DOM
- Хранение различных объектов, не связанных с JSX

---

### Как лучше использовать ref

- Рассматривайте ref как механизм внешнего доступа
- Не считывайте значение ref при render

---

### Ref и DOM

```typescript
<div ref={myRef}>
```

---


### Резюме

<small><ul>
<li>Ref это механизм внешнего доступа, позволяющий хранить значение, не используемые во время render.</li>
<li>Ref это обычный объект со свойством <code>current</code></li>
<li>Ref создается с помощью <code>useRef</code></li>
<li>Ref сохраняет информацию между render как и state</li>
<li>Ref не вызывает re-render, в отличие от state</li>
<li>Нельзя считывать <code>ref.current</code></li>
<ul><small>

