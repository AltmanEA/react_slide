### Вынос логики состояний в reducer

- **Переход** от установки состояния к диспетчеризации действий.
- **Описание** логики состояний в reducer.
- **Применение** reducer к состоянию.

---

### [Пример со сложной логикой](ex1)

---

### **Переход** от установки состояния к диспетчеризации действий.

<div style="display: flex;">
    <div style="flex: 2;">
<pre><code>function handleAddTask(text) {
  setTasks([
    ...tasks,
    {
      id: nextId++,
      text: text,
      done: false,
    },
  ]);
}</code></pre>
    </div>
    <div style="flex: 2;">
<pre><code>function handleAddTask(text) {
  dispatch({
    type: 'added',
    id: nextId++,
    text: text,
  });
}</code></pre>
    <div>
<div>

---

### **Описание** логики состояний в reducer.

```
function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
```

---

### **Применение** reducer к состоянию.

```
const [tasks, setTasks] = useState(initialTasks);
```
```
const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
```

---

### [Полный пример с reducer](ex2)

---

### Сравнение ```useReducer``` и ```useState```

- Размер кода
- Читаемость
- Отладка
- Тестирование
- Персональные предпочтения

---

### Правильная реализация reducer

- Reducer должен быть чистой функцией.
- Каждое action должно описывать одно действие пользователя, даже если оно ведет к изменению нескольких данных.

---

### [Написание reducer с помощью Immer](ex3)

```
const [tasks, dispatch] = 
    useImmerReducer(tasksReducer, initialTasks);
```
```
function tasksReducer(draft, action) {
  switch (action.type) {
    case 'added': {
      draft.push({
        id: action.id,
        text: action.text,
        done: false,
      });
      break;
    }
```

---

### Резюме

- <small>Чтобы перейти от useState к useReducer:</small>
    - <small>Переместите действия из обработчиков событий.</small>
    - <small>Напишите reducer, который возвращает следующее состояние для заданного состояния и действия. </small>
    - <small>Замените useState на useReducer.</small>
- <small>Reducer`ы требуют написания большего количества кода, но они помогают при отладке и тестировании.</small>
- <small>Reducer`ы должны быть чистыми.</small>
- <small>Каждое действие описывает одно взаимодействие с пользователем.</small>
- <small>Используйте Immer, если вы хотите писать reducer в мутирующем стиле</small>