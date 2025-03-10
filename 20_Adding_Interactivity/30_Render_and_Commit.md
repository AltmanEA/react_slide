### Рендер и коммит

<div style="display: flex; flex-wrap: wrap;">
  <div style="flex: 1; max-width: 33.33%;">
     <img src='i_render-and-commit1.png'/>
     Trigger
  </div>
  <div style="flex: 1; max-width: 33.33%;">
    <img src='i_render-and-commit2.png'/>
    Render
  </div>
  <div style="flex: 1; max-width: 33.33%;">
    <img src='i_render-and-commit3.png'/>
    Commit
  </div>  
</div>


---

### 1. Trigger a render. Initial render 

```
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

---

### 1. Trigger a render. Re-renders 

<div style="display: flex; flex-wrap: wrap;">
  <div style="flex: 1; max-width: 33.33%;">
     <img src='i_rerender1.png'/>
    State update...
  </div>
  <div style="flex: 1; max-width: 33.33%;">
    <img src='i_rerender2.png'/>
    ...triggers...
  </div>
  <div style="flex: 1; max-width: 33.33%;">
    <img src='i_rerender3.png'/>
    ...render!
  </div>  
</div>


---

### Step 2: React renders your components

```
export default function Gallery() {
  return (
    <section>
      <h1>Inspiring Sculptures</h1>
      <Image />
      <Image />
      <Image />
    </section>
  ); }
```
- Начальный render
- Rerender

---

### Step 3: React commits changes to the DOM 

- Начальный render - создает DOM
- Rerender - вносит минимально необходимые изменения