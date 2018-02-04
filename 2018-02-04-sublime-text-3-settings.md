---
layout: post
title: Sublime Text 3 Settings
tags: [sublime-text-3, python]
---

# OS Configuration

## Windows OS

1. In *system variables*
    ![](/images/sublime-text-3-system-variables.PNG "system environment variables")
2. Add `%SUBLIME%` to `PATH` in *user variables*.

# Packages

- Anaconda
- Git Commit Message Syntax
- GitGutter
- SideBarEnhancements
- SublimeLinter
- [SublimeLinter-flake8](https://github.com/SublimeLinter/SublimeLinter-flake8)
- Theme `-` SoDaReloaded
- Tomorrow Color Schemes
- Zen Tabs

## Anaconda

```
{
    "anaconda_linting": false,
    "pep8": false
}
```

## Git Commit Message

```
{
    "rulers": [50, 72]
}
```

## SublimeLinter

```
{
    "user": {
        "debug": false,
        "delay": 0.25,
        "error_color": "D02000",
        "gutter_theme": "Packages/SublimeLinter/gutter-themes/Default/Default.gutter-theme",
        "gutter_theme_excludes": [],
        "lint_mode": "load/save",
        "linters": {
            "flake8": {
                "@disable": false,
                "args": [],
                "builtins": "",
                "excludes": [],
                "executable": "",
                "ignore": "",
                "jobs": "1",
                "max-complexity": -1,
                "max-line-length": null,
                "select": "",
                "show-code": false
            }
        },
        "mark_style": "squiggly underline",
        "no_column_highlights_line": false,
        "passive_warnings": false,
        "paths": {
            "linux": [],
            "osx": [],
            "windows": []
        },
        "python_paths": {
            "linux": [],
            "osx": [],
            "windows": []
        },
        "rc_search_limit": 3,
        "shell_timeout": 10,
        "show_errors_on_save": false,
        "show_marks_in_minimap": true,
        "syntax_map": {
            "html (django)": "html",
            "html (rails)": "html",
            "html 5": "html",
            "javascript (babel)": "javascript",
            "magicpython": "python",
            "php": "html",
            "python django": "python",
            "pythonimproved": "python"
        },
        "tooltip_fontsize": "1rem",
        "tooltip_theme": "Packages/SublimeLinter/tooltip-themes/Default/Default.tooltip-theme",
        "tooltip_theme_excludes": [],
        "tooltips": false,
        "use_current_shell_path": false,
        "warning_color": "DDB700",
        "wrap_find": true
    }
}
```

## Zen Tabs

```
{
    "open_tab_limit": 5
}
```

# Sublime Text 3 User Settings

```
{
    "caret_style": "solid",
    "color_scheme": "Packages/User/SublimeLinter/Tomorrow-Night (SL).tmTheme",
    "draw_white_space": "all",
    "ensure_newline_at_eof_on_save": true,
    "file_exclude_patterns":
    [
        "*.pyc",
        "*.pyo",
        "*.exe",
        "*.dll",
        "*.obj",
        "*.o",
        "*.a",
        "*.lib",
        "*.so",
        "*.dylib",
        "*.ncb",
        "*.sdf",
        "*.suo",
        "*.pdb",
        "*.idb",
        ".DS_Store",
        "*.class",
        "*.psd",
        "*.db",
        "*.sublime-workspace"
    ],
    "fold_buttons": false,
    "folder_exclude_patterns":
    [
        ".svn",
        ".git",
        ".hg",
        "CVS",
        "__pycache__"
    ],
    "font_face": "Ubuntu Mono",
    "font_options":
    [
        "directwrite",
        "subpixel_antialias",
        "no_bold",
        "no_italic"
    ],
    "font_size": 12,
    "highlight_line": true,
    "highlight_modified_tabs": true,
    "ignored_packages":
    [
        "Vintage"
    ],
    "indent_guide_options":
    [
        "draw_active"
    ],
    "line_padding_bottom": 0,
    "line_padding_top": 0,
    "scroll_past_end": true,
    "shift_tab_unindent": true,
    "show_minimap": false,
    "tab_size": 4,
    "theme": "SoDaReloaded Dark.sublime-theme",
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true,
    "wide_caret": true
}
```

# Syntax Specific Settings

## Python

```
{
    // PEP 8 line-length guides & word wrapping
    "rulers": [72, 79],
    "word_wrap": true,
    "wrap_width": 80
}
```

