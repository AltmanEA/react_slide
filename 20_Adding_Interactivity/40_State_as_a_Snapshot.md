### Setting state triggers renders

[Пример](ex1)

---

### Rendering создает snapshot

<div style="display: flex; flex-wrap: wrap;">
<div style="flex: 1; max-width: 33.33%;">
<img src='i_render1.png' width=300/>
React executing the function    
</div><div style="flex: 1; max-width: 33.33%;">
<img src='i_render2.png' width=300/>
Calculating the snapshot
</div><div style="flex: 1; max-width: 33.33%;">
<img src='i_render3.png' width=300/>
Updating the DOM tree
</div></div>

---

### State находится в snapshot

<div style="display: flex; flex-wrap: wrap;">
<div style="flex: 1; max-width: 33.33%;">
<img src='i_state-snapshot1.png' width=300/>
You tell React to update the state  
</div><div style="flex: 1; max-width: 33.33%;">
<img src='i_state-snapshot2.png' width=300/>
React updates the state value
</div><div style="flex: 1; max-width: 33.33%;">
<img src='i_state-snapshot3.png' width=300/>
React passes a snapshot of the state value into the component
</div></div>

---

### Setting state only changes it for the next render. 

[Пример1](ex2)

[Пример2](ex3)

[Пример3](ex4)

---

### A state variable’s value never changes within a render

[Пример](ex5)