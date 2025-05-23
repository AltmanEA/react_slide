
### Состояние привязано к позиции в дереве

[Пример](ex1)

<div style="display: flex;">
    <div style="flex: 2;">
    <pre><code>const counter = < Counter />;
return (
    < div >
        {counter}
        {counter}
    < /div >
);
    </code></pre>
    </div>
    <div style="flex: 2;">
    <img src="preserving_state_tree.webp"/>
    <div>
<div>

---

### Состояние компонентов изолировано друг от друга

[Пример](ex2)

---

### Состояние сбрасывается при удалении компонента

[Пример](ex3)

---

### Тот же компонент в той же позиции сохраняет состояние

[Пример](ex4)

---

### Разные компоненты в одной и той же позиции сбрасывают состояние

[Пример](ex5)

---

### Проблема с вложенными компонентами


[Пример](ex6)

---

### Сброс состояния в одной и той же позиции

[Пример](ex7)

---

### Сброс состояния в одной и той же позиции

#### Рендер компонентов в разной позиции

[Пример](ex8)

---

### Сброс состояния в одной и той же позиции

#### Использование ключа

[Пример](ex9)

---

### Сброс формы с помощью ключа

[Проблема](ex10)

[Решение](ex11)

---

### Резюме

- <small>React сохраняет состояние до тех пор, пока один и тот же компонент отображается в одной и той же позиции.</small>
- <small>Состояние не хранится в JSX-тегах. Оно связано с позицией дерева, в которую вы поместили JSX.</small>
- <small>Вы можете заставить поддерево сбросить свое состояние, задав ему другой ключ.</small>
- <small>Не вставляйте определения компонентов, иначе вы случайно сбросите состояние.</small>
