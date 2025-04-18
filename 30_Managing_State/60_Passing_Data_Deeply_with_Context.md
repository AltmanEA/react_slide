### Проблема передачи props

![](passing_data_prop_drilling.webp)

---

### Передача свойств с помощью контекста

![](passing_data_context_far.webp)

---

### [Пример проблемы](ex1)

- Компонент ```Heading```
- Иерархическая структура
- Дублирование свойства

---

### [Пример контекста](ex2)

- Создание контекста
- Чтение контекста
- Установка контекста

---

### [Усложненный пример](ex3)

---

### [Передача контекста сквозь компоненты](ex4)

---

### Правила использования контекста

- используйте контекст только по необходимости
- передача многих props - не страшно
- проброс сквозь компонент часто лучше заменить передачей дочернего компонента

```<Layout posts={posts} />```

```<Layout><Posts posts={posts} /></Layout>```

---

### Варианты использования контекста

- создание тем
- информация об аккаунте
- маршрутизация
- управление состоянием

---

### Резюме

<small><ul>
    <li>Контекст позволяет передавать данные сразу многим компонентам в дереве компонентов</li>
    <li>Работа с контекстом:<ol>
        <li>Создать контекст</li>
        <li>Установить контекст</li>
        <li>Прочитать контекст</li>
    </ol></li>
    <li>Контекст может передавать данные сквозь компоненты</li>
    <li>Контекст позволяет писать компоненты, которые "адаптируются" к их "окружению"</li>
    <li>Перед использованием контекста попробуйте другие методы</li>
</ul></small>
