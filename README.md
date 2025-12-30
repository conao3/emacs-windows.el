# emacs-windows.el

Window configuration management and session persistence for GNU Emacs.

This package provides two complementary components:

- **windows.el** - Manage multiple named window configurations and switch between them instantly
- **revive.el** - Save and restore your complete Emacs editing session, including window layouts

## Overview

When working in Emacs, you often build up carefully arranged window layouts for different tasks. Reading mail might use a two-pane layout, while coding might split the screen into three or four windows. Switching between modes typically destroys these layouts.

This package solves that problem by letting you save multiple window configurations and switch between them with a single keystroke. Combined with session persistence, you can quit Emacs and resume exactly where you left off.

## Installation

Place `windows.el` and `revive.el` in your load path and add the following to your init file:

```elisp
(require 'windows)
(win:startup-with-window)
(define-key ctl-x-map "C" 'see-you-again)
```

## Usage

### Window Management

The default prefix key is `C-c C-w`. Common operations:

| Key Binding     | Description                                |
|-----------------|--------------------------------------------|
| `C-c C-w 1`     | Switch to window configuration 1           |
| `C-c C-w 2`     | Switch to window configuration 2           |
| `C-c C-w 0`     | Select unallocated frame or swap windows   |
| `C-c C-w SPC`   | Switch to previously shown configuration   |
| `C-c C-w C-n`   | Switch to next window                      |
| `C-c C-w C-p`   | Switch to previous window                  |
| `C-c C-w !`     | Delete current window configuration        |
| `C-c C-w =`     | Show window list                           |
| `C-c C-w C-w`   | Window operation menu                      |
| `C-c C-w C-r`   | Resume menu                                |
| `C-c C-w C-s`   | Switch task                                |

For faster access, quick selection keys `C-c 1` through `C-c 9` are available by default. Set `win:quick-selection` to `nil` to disable them.

### Creating Window Configurations

When switching to an unassigned window number, a creation menu appears:

```
C)reate D)uplicate P)reserve F)indfile B)uff X)M-x k)KeyFunc&exit N)o:
```

- **Create** - Start with a fresh single-window layout
- **Duplicate** - Copy the current window configuration
- **Preserve** - Save current layout to the new slot without switching
- **Findfile** - Open a file in the new configuration
- **Buff** - Select a buffer for the new configuration
- **M-x** - Execute a command in the new configuration

### Session Persistence

If you are using `windows.el`, session saving is integrated. Otherwise, configure `revive.el` separately:

```elisp
(autoload 'save-current-configuration "revive" "Save status" t)
(autoload 'resume "revive" "Resume Emacs" t)
(autoload 'wipe "revive" "Wipe Emacs" t)

(define-key ctl-x-map "S" 'save-current-configuration)
(define-key ctl-x-map "F" 'resume)
(define-key ctl-x-map "K" 'wipe)
```

- `C-x S` - Save current editing status
- `C-x F` - Resume saved status
- `C-x K` - Close all buffers

Use numeric prefixes to maintain multiple saved sessions:

```
C-u 2 C-x S    ; Save to slot 2
C-u 2 C-x F    ; Restore from slot 2
```

## Features

### Window Configuration

- Manage up to 10 (or more) named window configurations
- Instant switching between configurations
- Frame support with size and position restoration
- Integration with session persistence

### Session Restoration

- Complete window layout restoration
- Buffer positions and marks preserved
- Support for special modes: Dired, Shell, MH-E, Mew, Gnus, and more
- Global and local variable persistence
- Remote file support via ange-ftp

## Customization

### Window Configuration Variables

```elisp
;; Disable quick selection keys (C-c 1, C-c 2, etc.)
(setq win:quick-selection nil)

;; Change the prefix key
(setq win:switch-prefix "\C-cw")
```

### Session Persistence Variables

```elisp
;; Configuration file location
(setq revive:configuration-file "~/.revive.el")

;; Additional global variables to save
(setq revive:save-variables-global-private
      '(file-history buffer-history))

;; Additional local variables to save
(setq revive:save-variables-local-private
      '(my-local-var))

;; Buffers to ignore (regexp)
(setq revive:ignore-buffer-pattern "^ \\*")
```

### Mode-Specific Restoration

Define custom restoration commands for specific major modes:

```elisp
(setq revive:major-mode-command-alist-private
      '((my-custom-mode . my-custom-restore-function)
        ("*Special Buffer*" . special-buffer-command)))
```

## Requirements

- GNU Emacs 19 or later (frame support)
- Also works with Emacs 18 in terminal mode

## License

- **revive.el** - 2-Clause BSD License
- **windows.el** - Free software

## Author

HIROSE Yuuji <yuuji@gentei.org>

## See Also

This package provides functionality beyond what `desktop.el` and `saveconf.el` offer, particularly the ability to save and restore window splitting configurations.
