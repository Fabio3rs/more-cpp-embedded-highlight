# C++ Raw String Embedded Language Injection Grammar

This grammar enables syntax highlighting for embedded languages within C++ raw strings by leveraging TextMate grammar injection rules. It's designed for editors like VSCode, Atom, and Sublime Text that support TextMate-style syntax definitions.

## Why This Exists

This is a personal project to improve the editing experience for C++ developers.
C++ raw strings (of the form `R"delim(...)delim"`) are commonly used to embed:
- Web content (HTML/CSS/JS)
- Configuration files (JSON/YAML/XML)
- Scripts (Python/Shell)
- Documentation (Markdown)
- Other programming languages

Without this grammar, editors treat all raw string content as plain text. This solution automatically applies appropriate syntax highlighting based on the raw string's delimiter.

## Features

**Supported Languages**:
- Web: HTML, CSS, JavaScript
- Scripting: Python, PHP, Lua, Shell (sh/bash/zsh)
- Configuration: JSON, YAML, TOML, XML
- Documentation: Markdown
- Native: C, C++, Rust, Assembly
- Build Systems: CMake, Makefile, Dockerfile
- And more (see full list below)

**Automatic Detection**:
```cpp
// Highlights as HTML
std::string html = R"html(<div class="test">${value}</div>)html";

// Highlights as JavaScript
std::string js = R"js(console.log("Hello from C++!");)js";

// Highlights as Python
std::string py = R"py(def hello(): print("Hello World"))py";
```

## How It Works

The grammar uses these key components:

1. **Injection Selector**:
   ```json
   "injectionSelector": "L:source.cpp -string -comment"
   ```
   Applies to C++ files while avoiding conflicts with existing string/comment parsing.

2. **Pattern Matching**:
   ```json
   {
     "begin": "R\"html\\(",
     "end": "\\)html\"",
     "contentName": "text.html.basic",
     "patterns": [{"include": "text.html.basic"}]
   }
   ```
   - `begin`/`end`: Match raw string delimiters
   - `contentName`: Sets base syntax context
   - `patterns`: Inherits full language grammar

## Full List of Supported Languages

| Delimiter    | Language     | Scope Name              |
|--------------|--------------|-------------------------|
| html         | HTML         | `text.html.basic`       |
| js           | JavaScript   | `source.js`             |
| css          | CSS          | `source.css`            |
| py           | Python       | `source.python`         |
| xml          | XML          | `text.xml`              |
| json         | JSON         | `source.json`           |
| yaml         | YAML         | `source.yaml`           |
| toml         | TOML         | `source.toml`           |
| md           | Markdown     | `text.html.markdown`    |
| sh/bash/zsh  | Shell        | `source.shell`          |
| lua          | Lua          | `source.lua`            |
| rust         | Rust         | `source.rust`           |
| dockerfile   | Dockerfile   | `source.dockerfile`     |
| ...          | ...          | ...                     |

## Installation

1. **For VSCode**:
   Place in `.vscode/syntaxes/cpp-raw-strings.json` and add to `package.json`:
   ```json
   "contributes": {
     "grammars": [{
       "scopeName": "inline.cpp-html-injection",
       "path": "./syntaxes/cpp-raw-strings.json"
     }]
   }
   ```

2. **For Other Editors**:
   Add to your editor's C/C++ syntax definitions folder.

## Limitations

1. Requires target language grammars to be installed
2. May conflict with other injection grammars
3. Delimiters are case-sensitive (`R"HTML(` ≠ `R"html(`)

## Customization

To add new languages:

1. Add a new pattern block:
   ```json
   {
     "begin": "R\"newlang\\(",
     "end": "\\)newlang\"",
     "contentName": "source.newlang",
     "patterns": [{"include": "source.newlang"}]
   }
   ```
2. Replace `newlang` with your delimiter
3. Update `contentName` and `include` to match target language scope

---

**Note**: This grammar works best when combined with proper C++ syntax highlighting and language-specific packages for embedded content.
