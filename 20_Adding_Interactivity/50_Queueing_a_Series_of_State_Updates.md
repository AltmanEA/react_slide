### Обработка обновлений состояния

![](i_react-batching.png)

---

### Обновление состояния несколько раз в одном обработчике

[Пример1](ex1)

[Пример2](ex2)

[Пример3](ex3)

---

### Соглашение о именовании переменных

```typescript
setEnabled(e => !e);
setLastName(ln => ln.reverse());
setFriendCount(fc => fc * 2);
```