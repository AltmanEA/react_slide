### Компонент - строительный блок интерфейса

<div style="display: flex; font-size: 0.5em;">
    <div style="flex: 2;">
    <img src="html_example.png"/>
    </div>
    <div style="flex: 2;">
    <img src="components_example.png"/>
    <div>
<div>

---

### [Пример компонента](ex1)

```
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

---

### [Пример использования компонента](ex2)

```
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

---

### Вложенные компоненты

```
export default function Gallery() {
  // 🔴 Never define a component inside another component!
  function Profile() {
    // ...
  }
  // ...
}