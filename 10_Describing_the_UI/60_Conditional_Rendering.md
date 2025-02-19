### Условный рендеринг

```
function Item(
    { name, isPacked }: 
    { name: string, isPacked: boolean }
) {
    if (isPacked) {
      return <li className="item">{name} ✅</li>;
    }
    return <li className="item">{name}</li>;
}
```
[Пример](ex1)

---

### JSX null

```
function Item(
  { name, isPacked }: 
  { name: string, isPacked: boolean }
) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}
```
[Пример](ex2)

---

### Условные JSX, оператор ?:

```
function Item(
  { name, isPacked }:
    { name: string, isPacked: boolean }
) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✅'}
        </del>
      ) :  name }
    </li>
  );
}
```
[Пример](ex3)

---

### Условные JSX, оператор &&

```
function Item(
  { name, isPacked }:
    { name: string, isPacked: boolean }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}
```
[Пример](ex4)

---

### Условные JSX, условные переменные

```
function Item(
  { name, isPacked }:
    { name: string, isPacked: boolean }
) {
  let itemContent = name;
  if (isPacked) {
    itemContent = name + " ✅";
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}
```
[Пример](ex5)