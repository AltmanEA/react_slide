### Обычное веб-приложение

![](writing_jsx_html.webp)
![](writing_jsx_js.webp)

---

### Приложение на react

![](writing_jsx_sidebar.webp)
![](writing_jsx_form.webp)

---

### Преобразование HTML в JSX

```
<h1>Hedy Lamarr's Todos</h1>
<img 
    src="https://i.imgur.com/yXOvdOSs.jpg" 
    alt="Hedy Lamarr" 
    class="photo"
>
<ul>
    <li>Invent new traffic lights
    <li>Rehearse a movie scene
    <li>Improve the spectrum technology
</ul>
```

---

### В JSX должен быть один корневой элемент

```
JSX expressions must have one parent element.
```
```
<>
    <h1>Hedy Lamarr's Todos</h1>
    <img 
        src="https://i.imgur.com/yXOvdOSs.jpg" 
        alt="Hedy Lamarr" 
        class="photo"
    >
    <ul>
        <li>Invent new traffic lights
        <li>Rehearse a movie scene
        <li>Improve the spectrum technology
    </ul>
</>
```

---

### Все теги должны быть закрыты

```
JSX fragment has no corresponding closing tag.
```
```
<>
    <h1>Hedy Lamarr's Todos</h1>
    <img 
        src="https://i.imgur.com/yXOvdOSs.jpg" 
        alt="Hedy Lamarr" 
        class="photo"
    />
    <ul>
        <li>Invent new traffic lights</li>
        <li>Rehearse a movie scene</li>
        <li>Improve the spectrum technology</li>
    </ul>
</>
```

---

### Зарезервированные слова заменяются синонимами

```
_ Property 'class' does not exist on type _ Did you mean 'className'?
```
```
<>
    <h1>Hedy Lamarr's Todos</h1>
    <img 
        src="https://i.imgur.com/yXOvdOSs.jpg" 
        alt="Hedy Lamarr" 
        className="photo" 
    />
    <ul>
        <li>Invent new traffic lights</li>
        <li>Rehearse a movie scene</li>
        <li>Improve the spectrum technology</li>
    </ul>
</>
```

---

### [HTML to JSX конвертер](https://transform.tools/html-to-jsx)

