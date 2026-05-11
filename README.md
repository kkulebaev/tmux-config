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

| Option           | Value | Effect |
| ---------------- | ----- | ------ |
| `mouse`          | `on`  | Колесо мыши скроллит вывод (входит в copy mode), клик переключает панель, граница тащится мышью, выделение копирует в буфер |
| `history-limit`  | 50000 | Размер буфера прокрутки каждой панели — 50k строк вместо дефолтных 2000 |
| `set-clipboard`  | `on`  | Копирование из tmux уходит в системный буфер GNOME через OSC 52 (поддерживается VTE-терминалами: gnome-terminal, ddterm) |

## Installation

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

Все стандартные. Префикс — `Ctrl+b`.

| Keys           | Action |
| -------------- | ------ |
| `Ctrl+b ?`     | Полный список биндингов |
| `Ctrl+b [`     | Войти в copy mode (скролл, поиск, копирование) |
| `Ctrl+b c`     | Новое окно |
| `Ctrl+b %` / `"` | Сплит панели вертикально / горизонтально |
| `Ctrl+b z`     | Развернуть панель на всё окно (zoom) |
| `Ctrl+b d`     | Detach — отключиться от сессии, оставив её в фоне |

## Updating

```bash
cd ~/develop/tmux-config
git pull
tmux source-file ~/.tmux.conf
```
