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

```3/2/App2```

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

```3/2/App3```

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

```3/2/App5, 6, 7```

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

```3/2/App8, 9```
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

### Избегайте глубоко вложенного состояния

```3/2/App8, 9, 10```
```
const initialTravelPlan = {
    id: 0,
    title: '(Root)',
    childPlaces: [{
      id: 1,
      title: 'Earth',
      childPlaces: [{
        id: 2,
        title: 'Africa',
        childPlaces: [{
```

```
const initialTravelPlan: TravelPlanType = {
    0: {
      id: 0,
      title: '(Root)',
      childIds: [1, 42, 46],
    }, ...
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
