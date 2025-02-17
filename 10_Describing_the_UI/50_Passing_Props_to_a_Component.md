### Предопределенные props

```
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}
export default function Profile() {
  return <Avatar />
}
```

---

### Определяемые props. Передача

```
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

---

### Определяемые props. Чтение

```
<Avatar
    person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
    size={100}
/>
```
```
function Avatar(
  { person, size }: 
  { person: { name: string, imageId: string }, size: number }
)
```
```
function Avatar(
  props: 
  { person: { name: string, imageId: string }, size: number }
)
```
[Пример](ex1)

---

### Задание props по умолчанию

```
function Avatar(
  { person, size=100 }: 
  { person: { name: string, imageId: string }, size: number }
)
```

---

### Передача списка props 

```
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```
```
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

---

### Передача JSX в props 

```
<Card>
  <Avatar
    size={100}
    person={{ 
      name: 'Katsuko Saruhashi',
      imageId: 'YfeOqp2'
    }}
  />
</Card>
```
[Пример](ex2)

---

### Изменение props 

[Пример](ex3)