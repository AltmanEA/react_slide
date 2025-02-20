### Рендеринг списка с помощью map

```
export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```
![](key_warning.png)

[Пример](ex1)

---

### Корректный рендеринг списка

```
export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
    ...
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

[Пример](ex2)

---

### Правила для key

- Key должен быть уникален среди sibling.
- Key не должен изменяться

---

### Откуда брать key

- Для данных из источников данных можно использовать идентификатор
- Для данных, получаемых в приложении, можно использовать UUID.

---

### Фильтрация элементов списка

```
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
const listItems = chemists.map(person =>
```

[Пример](ex3)
