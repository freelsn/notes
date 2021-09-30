---
title: VSCode Settings
---

```json
{
    "remote.SSH.remotePlatform": {
        "lnx-snliu": "linux"
    },
    "terminal.integrated.defaultProfile.windows": "Git Bash",
    "workbench.editorAssociations": {
        "*.ipynb": "jupyter.notebook.ipynb"
    },
    "editor.wordWrap": "on",
    "editor.dragAndDrop": false,
    "editor.detectIndentation": false,
    "editor.minimap.enabled": false,
    "editor.suggest.insertMode": "replace",
    "[python]": {
        "editor.wordBasedSuggestions": false,
        "editor.rulers": [72, 80]
    },
    "[cpp]": {
        "editor.rulers": [80, 120]
    },
    "[c]": {
        "editor.rulers": [80, 120]
    },
    "files.trimFinalNewlines": true,
    "files.trimTrailingWhitespace": true,
    "files.insertFinalNewline": true,
    "files.autoGuessEncoding": true,
    "workbench.colorCustomizations": {
        "terminal.foreground": "#ffffff",
        "terminal.background": "#333333",
        "terminal.ansiBlack": "#4d4d4d",
        "terminal.ansiBlue": "#cd853f",
        "terminal.ansiCyan": "#ffa0a0",
        "terminal.ansiGreen": "#98fb98",
        "terminal.ansiMagenta": "#ffdead",
        "terminal.ansiRed": "#bb0000",
        "terminal.ansiWhite": "#f5deb3",
        "terminal.ansiYellow": "#f0e68c",
        "terminal.ansiBrightBlack": "#555555",
        "terminal.ansiBrightBlue": "#87ceeb",
        "terminal.ansiBrightCyan": "#ffd700",
        "terminal.ansiBrightGreen": "#55ff55",
        "terminal.ansiBrightMagenta": "#ff55ff",
        "terminal.ansiBrightRed": "#ff5555",
        "terminal.ansiBrightWhite": "#ffffff",
        "terminal.ansiBrightYellow": "#ffff55",
        "terminal.selectionBackground": "#066606",
        "terminalCursor.foreground": "#00ff00",
    },
    "security.workspace.trust.untrustedFiles": "open",
    "gitlens.currentLine.enabled": false,
    "gitlens.hovers.currentLine.over": "line",
    "[html]": {
        "editor.tabSize": 2,
    },
    "[javascript]": {
        "editor.tabSize": 2
    },
    "[css]": {
        "editor.tabSize": 2
    },
    "markdown-preview-enhanced.previewTheme": "github-light.css",
    "[matlab]": {
        "editor.rulers": [80, 120]
    }
}
```