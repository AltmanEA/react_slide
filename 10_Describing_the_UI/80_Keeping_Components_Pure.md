### Чистая (pure) функция

- Занимается только своими делами
- То же вход - то же выход.

<div style="display: flex;">
    <div style="flex: 2;">
    Математическая формула
    <pre><code>y=2*x</pre></code>
    </div>
    <div style="flex: 2;">
    Функция в программе
    <pre><code>function double(number) {
  return 2 * number;
}</pre></code><div>
<div>

---

### Компоненты react должны быть чистыми функциями

```
function Recipe({ drinkers }) {
  return (
    <ol>    
      <li>Boil {drinkers} cups of water.</li>
      <li>Add {drinkers} spoons of tea and {0.5 * drinkers} spoons of spice.</li>
      <li>Add {0.5 * drinkers} cups of milk to boil and sugar to taste.</li>
    </ol>
  );
}
```

---

### Компонент не должен иметь побочных эффектов 

[Пример](ex1)
```
let guest = 0;
function Cup() {
  // Bad: changing a preexisting variable!
  guest = guest + 1;
  return <h2>Tea cup for guest #{guest}</h2>;
}
export default function TeaSet() {
  return <> <Cup /> <Cup /> <Cup /> </>
}
```
```
Tea cup for guest #2
Tea cup for guest #4
Tea cup for guest #6
```

---

### Компонент не должен иметь побочных эффектов

```
function Cup({ guest }) {
    return <h2>Tea cup for guest #{guest}</h2>;
}
export default function TeaSet() {
    return ( <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </> );
}
```
```
Tea cup for guest #1
Tea cup for guest #2
Tea cup for guest #3
```

---

### Локальные переменные

```
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}
export default function TeaGathering() {
  let cups = [];
  for (let i = 1; i <= 12; i++) {
    cups.push(<Cup key={i} guest={i} />);
  }
  return cups;
}
```

---

### Если необходимы побочные эффекты

- Обработчики событий (event handler)
- Эффекты (effect)