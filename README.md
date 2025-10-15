# agent-shell-sidebar

A persistent sidebar interface for [agent-shell](https://github.com/xenodium/agent-shell), providing a treemacs-like side panel for interacting with LLM agents.

![Demo](demo.webp)

## Features

- **Project-aware**: Each project gets its own independent sidebar with separate state
- **Persistent**: Sidebar state persists across visibility toggles
- **Configurable**: Flexible width (absolute or percentage), position (left/right), and locking behavior
- **Responsive**: Locked sidebars automatically adjust to frame size changes

## Installation

### Dependencies

- Emacs 29.1+
- [agent-shell](https://github.com/xenodium/agent-shell) 0.5.1+

### Using `use-package`

```emacs-lisp
(use-package agent-shell-sidebar
  :after agent-shell
  :vc (:url "https://github.com/cmacrae/agent-shell-sidebar"))
```

### Manual Installation

Clone the repository and add to your load path:

```emacs-lisp
(add-to-list 'load-path "/path/to/agent-shell-sidebar")
(require 'agent-shell-sidebar)
```

## Usage

### Commands

- `M-x agent-shell-sidebar-toggle` - Toggle sidebar visibility for current project
- `M-x agent-shell-sidebar-toggle-focus` - Toggle focus between sidebar and last buffer
- `M-x agent-shell-sidebar-change-provider` - Switch to a different agent provider
- `M-x agent-shell-sidebar-reset` - Reset sidebar for current project

### Recommended Keybindings

```emacs-lisp
(global-set-key (kbd "C-c a s") #'agent-shell-sidebar-toggle)
(global-set-key (kbd "C-c a f") #'agent-shell-sidebar-toggle-focus)
```

## Configuration

### Basic Configuration

```emacs-lisp
;; Sidebar width (default: "25%")
;; Can be integer (columns) or string with % (percentage of frame)
(setq agent-shell-sidebar-width "30%")

;; Minimum width (default: 80)
(setq agent-shell-sidebar-minimum-width 100)

;; Maximum width (default: "50%")
(setq agent-shell-sidebar-maximum-width "40%")

;; Position: 'left or 'right (default: 'right)
(setq agent-shell-sidebar-position 'left)

;; Lock sidebar position and size (default: t)
;; When locked: fixed size, invisible to other-window (C-x o)
;; When unlocked: manually resizable, visible to other-window
(setq agent-shell-sidebar-locked nil)
```

### Default Provider Configuration

Set a default agent config to skip provider selection:

```emacs-lisp
;; Use Claude Code by default
(setq agent-shell-sidebar-default-config
      (agent-shell-anthropic-make-claude-code-config))

;; Or use Gemini CLI by default
(setq agent-shell-sidebar-default-config
      (agent-shell-google-make-gemini-config))

;; Or use a custom config
(setq agent-shell-sidebar-default-config
      (agent-shell-make-agent-config
       :mode-line-name "Custom Agent"
       :buffer-name "Custom"
       :shell-prompt "Custom> "
       :shell-prompt-regexp "Custom> "
       :client-maker #'my-custom-client-maker))
```

### Complete Example

```emacs-lisp
(use-package agent-shell-sidebar
  :after agent-shell
  :vc (:url "https://github.com/cmacrae/agent-shell-sidebar")
  :custom
  (agent-shell-sidebar-width "25%")
  (agent-shell-sidebar-minimum-width 80)
  (agent-shell-sidebar-maximum-width "50%")
  (agent-shell-sidebar-position 'right)
  (agent-shell-sidebar-locked t)
  (agent-shell-sidebar-default-config
   (agent-shell-anthropic-make-claude-code-config))
  :bind
  (("C-c a s" . agent-shell-sidebar-toggle)
   ("C-c a f" . agent-shell-sidebar-toggle-focus)))
```

## How It Works

### Project State Management

Each project root (detected via projectile or project.el) maintains independent state:

- **Buffer**: The agent-shell buffer for this project
- **Config**: The agent configuration being used
- **Last Buffer**: The last non-sidebar buffer with focus
- **Width**: The current width (when unlocked)

### Width Calculation

Width is calculated with constraints applied in order:

1. Parse configured width (`agent-shell-sidebar-width`)
2. Apply minimum constraint (`agent-shell-sidebar-minimum-width`)
3. Apply maximum constraint (`agent-shell-sidebar-maximum-width`)

If minimum > maximum, minimum takes precedence.

### Locked vs Unlocked Behavior

**Locked (default)**:
- Fixed width, recalculated from config on display
- Invisible to `other-window` (C-x o)
- Responsive to frame size changes

**Unlocked**:
- Manually resizable
- Width persists across visibility toggles
- Visible to `other-window` commands

## Contributing

Contributions welcome! Please open issues or pull requests on GitHub.

## License

GPL-3.0-or-later

## Credits

- **Álvaro Ramírez** - for the incredible [agent-shell](https://github.com/xenodium/agent-shell)

## See Also

- [agent-shell](https://github.com/xenodium/agent-shell) - The main agent-shell package
- [shell-maker](https://github.com/xenodium/shell-maker) - The underlying shell framework
- [acp.el](https://github.com/xenodium/acp.el) - Agent Client Protocol implementation
