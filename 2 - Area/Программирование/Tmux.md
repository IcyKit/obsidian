# Tmux

Tmux - Консольный мультиплексор, позволяющий работать в одном окне, с несколькими терминалами.
Для любого действия в tmux для начала нужно нажать шорткат, а потом уже комбинацию или одну клавишу.

По дефолту - `Ctrl + b`
У меня - `Ctrl + s`

### Создание новой сессии

```bash
tmux new-session -s *название сессии*
```

---
### Войти в существующую сессию

```bash
tmux attach -t *название сессии*
```

---
### Очистить сервер (закрыть все сессии)

```bash
tmux kill-server
```

---
### Отключиться от сессии

```bash
tmux detach
```

---
### Создать новое вертикальное окно

```bash
Шорткат, Shift + 5
```

---
### Создать новое горизонтальное окно

```bash
Шорткат, Shift + '
```

---
### Переключение между окнами

```bash
Шорткат, → / ← / ↑ / ↓
```

---
### Закрыть окно

```bash
Шорткат, x
# Или
exit # В терминале
```

---
### Быстрое отсоединение от сессии

```bash
Шорткат, d
```

---
### Графический выбор сессий.

```bash
Шорткат, w
```

---
### Переименование окна

```bash
Шорткат + ,
```