### Импорт и экспорт компонентов

[В одном файле](ex1)

[В разных файлах](ex2)

---

### Экспорт по умолчанию (default export)

```
export default function Button() {}
```
```
import Button from './Button';
```

---

### Именнованный экспорт (named export)

```
export function Button() {}
```
```
import { Button } from './Button';
```

---

### Смешанный экспорт

```
export function BigButton() {}
export default function Button() {}
```
```
import Button, { BigButton } from './Button';
```

---

### [Пример смешанного экспорта](ex3)