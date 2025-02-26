### Почему не подходят обычные переменные

- Не сохраняют данные между рендерингом
- Не вызывают рендрер при изменении

[Пример](ex1)

---

### Переменная - состояние (state)

```
const [index, setIndex] = useState(0);

function handleClick() {
  setIndex(index + 1);
}
```

[Пример](ex2)

---

### Хук (hook)

- Функция, цепляющая функциональность к компоненту
- Начинается с ```use```
- Может вызываться на верхнем уровне компонента или хука

---

### useState

```
const [index, setIndex] = useState(0);
```

- Первый ренден - устанавливается в начальное состояние.
- Обновление состояние - устанавливается в указанную величину.
- Ререндер - значение не изменяется.

---

### Несколько состояний

```
export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);

  function handleNextClick() { setIndex(index + 1); }

  function handleMoreClick() { setShowMore(!showMore); }
```

[Пример](ex3)

---

### У каждого компонента свои состояния

```
<div className="Page">
    <Gallery />
    <Gallery />
</div>
```

[Пример](ex4)
