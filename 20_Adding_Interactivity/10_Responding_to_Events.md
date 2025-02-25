### Добавление обработчика событий (event handler)
```
export default function Button() {
  return (
    <button>
      I don't do anything
    </button>
  );
}
```

- Создать функцию `handleClick` внутри компонента
- Реализовать логику внутри `handleClick`
- Добавить `onClick={handleClick}` в кнопку.

---

### Пример обработчика событий

```
export default function Button() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

[Пример](ex1)

---

### Свойства обработчика событий

- Внутри компонента
- Начинается с handle
- Можно определять внутри JSX:

```
<button onClick={() => {
  alert('You clicked me!');
}}>
```

---

### Использование обработчика событий

<div style="display: flex;">
  <div style="flex: 2;">
  Правильно:
    <img src="right.png"/>
  </div>
  <div style="flex: 2;">
  Неправильно:
    <img src="wrong.png"/>
  <div>
<div>

---

### Использование props в обработчике событий

```
function AlertButton({ message, children }) {
  return (
    <button onClick={() => alert(message)}>
      {children}
    </button>
  );
}
```

[Пример](ex2)

---

### Передача обработчика событий в props

```
function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}
```

[Пример](ex3)


---

### Имя обработчика событий

- Встроенные компоненты - ```onClick```
- Собственные компоненты - ```onXxxx```

[Пример](ex4)

[Пример](ex5)

---

### Распространение событий

Родительские компоненты по-умолчанию получают события дочерних.

[Пример](ex6)

---

### Запрет распространения событий

```
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
```

[Пример](ex7)


---

### Собственное распространения событий

```
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}
```

---

### Запрет поведения по-умолчанию

```
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>
    </form>
  );
}
```

[Без запрета](ex8)

[С запретом](ex9)

---

### Может ли обработчик событий иметь побочный эффект?

