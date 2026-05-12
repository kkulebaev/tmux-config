<pre align="center">
████████╗███╗   ███╗██╗   ██╗██╗  ██╗      ██████╗ ██████╗ ███╗   ██╗███████╗██╗ ██████╗ 
╚══██╔══╝████╗ ████║██║   ██║╚██╗██╔╝     ██╔════╝██╔═══██╗████╗  ██║██╔════╝██║██╔════╝ 
   ██║   ██╔████╔██║██║   ██║ ╚███╔╝      ██║     ██║   ██║██╔██╗ ██║█████╗  ██║██║  ███╗
   ██║   ██║╚██╔╝██║██║   ██║ ██╔██╗      ██║     ██║   ██║██║╚██╗██║██╔══╝  ██║██║   ██║
   ██║   ██║ ╚═╝ ██║╚██████╔╝██╔╝ ██╗     ╚██████╗╚██████╔╝██║ ╚████║██║     ██║╚██████╔╝
   ╚═╝   ╚═╝     ╚═╝ ╚═════╝ ╚═╝  ╚═╝      ╚═════╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝     ╚═╝ ╚═════╝ 
</pre>

<p align="center">
  <em>Personal tmux configuration for Fedora Linux — mouse, scrollback and system clipboard</em>
</p>

<p align="center">
  <img alt="Tool" src="https://img.shields.io/badge/tool-tmux-1BB91F?style=flat-square">
  <img alt="Version" src="https://img.shields.io/badge/tmux-3.5a-1BB91F?style=flat-square">
  <img alt="OS" src="https://img.shields.io/badge/OS-Fedora%20Linux-51A2DA?style=flat-square">
</p>

## Settings

| Option              | Value | Effect |
| ------------------- | ----- | ------ |
| `mouse`             | `on`  | Колесо мыши скроллит вывод (входит в copy mode), клик переключает панель, граница тащится мышью, выделение копирует в буфер |
| `history-limit`     | 50000 | Размер буфера прокрутки каждой панели — 50k строк вместо дефолтных 2000 |
| `set-clipboard`     | `on`  | Копирование из tmux уходит в системный буфер через OSC 52 (поддерживается VTE-терминалами: gnome-terminal, ddterm), работает в том числе по SSH |
| `allow-passthrough` | `on`  | Разрешает escape-последовательности из вложенных процессов (OSC 52, terminfo-запросы) |
| `renumber-windows`  | `on`  | После закрытия окна нумерация остальных уплотняется — не остаётся «дыр» в индексах |
| `mode-keys`         | `vi`  | В copy mode работают vim-биндинги: `hjkl`, `/`, `v` для выделения, `y` для копирования через `wl-copy` |

### Prefix indicator

Статус-бар окрашивается в оранжевый (`colour208`), пока нажат префикс `Ctrl+a` и tmux ждёт следующую клавишу. В остальное время — тёмно-серый (`colour235`). Реализовано через `status-style` с format-expression `#{?client_prefix,...,...}` — tmux пересчитывает стиль автоматически при срабатывании/сбросе префикса.

### Pane title bar

Сверху каждой панели рисуется тонкая полоска (`pane-border-status top`) с индексом и заголовком панели, выровненными по правому краю. Активная панель — голубой фон (`colour39`) с чёрным жирным текстом и голубой границей; неактивные — тёмно-серый фон с приглушённым текстом. Подсветка следует за курсором — видно, в какой панели сейчас фокус.

Заголовок панели задаётся через `Ctrl+a T` (prompt для ввода) либо командой `select-pane -T "имя"`. По умолчанию tmux берёт его из терминала (обычно hostname).

## Installation

Для копирования в системный буфер на Wayland нужен `wl-clipboard`:

```bash
sudo dnf install wl-clipboard
```

```bash
git clone git@github.com:kkulebaev/tmux-config.git ~/develop/tmux-config
ln -sf ~/develop/tmux-config/.tmux.conf ~/.tmux.conf
```

Применить к запущенному tmux-серверу:

```bash
tmux source-file ~/.tmux.conf
```

`history-limit` применится только к **новым** окнам и панелям — у уже открытых буфер прокрутки остаётся прежним.

## Key bindings

Префикс переопределён на `Ctrl+a` (вместо стандартного `Ctrl+b`). Чтобы передать literal `Ctrl+a` в приложение внутри tmux (например, в shell — переход в начало строки), нажми `Ctrl+a Ctrl+a`.

| Keys             | Action |
| ---------------- | ------ |
| `Ctrl+a h`       | Открыть личную шпаргалку в popup (см. [`cheatsheet.md`](cheatsheet.md)) |
| `Ctrl+a ?`       | Полный список биндингов |
| `Ctrl+a [`       | Войти в copy mode (скролл, поиск, копирование) |
| `Ctrl+a c`       | Новое окно |
| `Ctrl+a %` / `"` | Сплит панели вертикально / горизонтально |
| `Ctrl+a T`       | Переименовать текущую панель (`select-pane -T`) |
| `Ctrl+a z`       | Развернуть панель на всё окно (zoom) |
| `Ctrl+a d`       | Detach — отключиться от сессии, оставив её в фоне |

`cheatsheet.md` — личная шпаргалка, которая пополняется по мере выработки мышечной памяти. Открывается через `display-popup` поверх tmux, читается в `less` (`q` — выход, `/` — поиск). Путь к файлу зашит в биндинг как `$HOME/develop/tmux-config/cheatsheet.md`.

### Copy mode

После `Ctrl+a [`:

| Keys           | Action |
| -------------- | ------ |
| `h` `j` `k` `l` | Перемещение курсора |
| `/` / `?`      | Поиск вперёд / назад |
| `v`            | Начать выделение |
| `y`            | Скопировать выделенное в системный буфер через `wl-copy` и выйти |
| `q`            | Выйти из copy mode |

Выделение мышью (зажать ЛКМ → отпустить) тоже копирует в системный буфер.

## Updating

```bash
cd ~/develop/tmux-config
git pull
tmux source-file ~/.tmux.conf
```
