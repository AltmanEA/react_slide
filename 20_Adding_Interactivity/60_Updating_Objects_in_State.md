### Понятие мутации

```typescript
const [x, setX] = useState(0);
setX(5);
```
```typescript
const [position, setPosition] = useState({ x: 0, y: 0 });
position.x = 5;
```

---

### Состояние нужно рассматривать как только-для-чтения

[Пример1](ex1)

[Пример2](ex2)

---

### Использование typescript для контроля за мутациями

```typescript
const [array, setArray] = useState<readonly string[]>(['a']);
const onClick = () => {
  setArray((array) => {
    // Type error: Property 'push' does not exist 
    //   on type 'readonly string[]'.
    array.push('b');
    return array;
  });
};
```

---

### Использование оператора расширения

[Пример1](ex3)

[Пример2](ex4)


---

### Обновление вложенных объектов

[Пример](ex5)


---

### Обновление вложенных объектов с помощью Immer

[Пример](ex6)