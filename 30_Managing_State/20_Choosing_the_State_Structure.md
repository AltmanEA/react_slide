### Принципы структурирования состояния

- Группируйте связанные состояния. 
- Избегайте противоречий в состоянии.
- Не помещайте в состояние то, что можно вычислить из props или других состояний.
- Сократите дублирование информации в различных компонентах, когда это возможно.
- Избегайте глубоко вложенного состояния.

---

### Группируйте связанные состояния

```
const [x, setX] = useState(0);
const [y, setY] = useState(0);
```
```
const [position, setPosition] = useState({ x: 0, y: 0 });
```

---

### Избегайте противоречий в состоянии

[Пример](ex1)

```
const [isSending, setIsSending] = useState(false);
const [isSent, setIsSent] = useState(false);
```

```
const [status, setStatus] = useState('typing');
...
const isSending = status === 'sending';
const isSent = status === 'sent';
```

---

### Избегайте избыточного состояния

[Пример](ex2)

```
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState('');
```
```
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');

const fullName = firstName + ' ' + lastName;
```

---

### Избегайте дублирования в состоянии

[Дублирование](ex3) [Проблема](ex4) [Решение](ex5)

```
const [items, setItems] = useState(initialItems);
const [selectedItem, setSelectedItem] = useState(
    items[0]
);
```
```
const [items, setItems] = useState(initialItems);
const [selectedId, setSelectedId] = useState(0);
const selectedItem = items.find(item =>
    item.id === selectedId ) ?? items[0];
```

---

### Избегайте глубоко вложенного состояния

 [Проблема](ex6) [Решение](ex7) [Расширение](ex8) 

```
export type PlaceType = {
    id: number,
    title: string,
    childPlaces: PlaceType[] }
export type TravelPlanType = {
    id: number,
    title: string,
    childPlaces: PlaceType[] }
```

```
export type PlaceType = {
    id: number
    title: string
    childIds: number[] }
export type TravelPlanType = {
    [key: number]: PlaceType }
```

---

### Резюме


- <small>Если две переменные состояния всегда обновляются вместе, подумайте о том, чтобы объединить их в одну.</small>
- <small>Тщательно выбирайте переменные состояния, чтобы избежать создания "невозможных" состояний.</small>
- <small>Структурируйте состояние таким образом, чтобы уменьшить вероятность ошибки при его обновлении.</small>
- <small>Избегайте избыточных и дублирующих состояний, чтобы не нужно было их синхронизировать.</small>
- <small>Не помещайте пропсы в состояние, если только вы специально не хотите предотвратить их обновление.</small>
- <small>Для шаблонов пользовательского интерфейса, таких как выбор, храните ID или индекс в состоянии, а не сам объект.</small>
- <small>Если обновление глубоко вложенного состояния является сложным, попробуйте сгладить его.</small>
