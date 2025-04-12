#  (Da'era) - Python Circular Dependency Analyzer

Da'era (Arabic for "circle") identifies circular dependencies in Python codebases by building and analyzing the import graph of your project. Circular dependencies occur when modules import each other in a loop, leading to potential issues like import errors, unpredictable initialization order, and tight coupling that makes maintenance difficult.

## Installation

```bash
# Clone the repository
git clone https://github.com/MohamedElashri/daera

# Change to project directory
cd daera

# Make the script executable
chmod +x daera

# Optional: create a symlink to use globally
sudo ln -s $(pwd)/daera /usr/local/bin/daera
```

## Usage

Basic usage:
```bash
daera /path/to/your/python/project
```

The tool requires specifying a project directory to analyze. Running without arguments shows the help menu.

### Command Line Options

| Option | Description |
|--------|-------------|
| `-h, --help` | Show help message and exit |
| `-v, --verbose` | Increase verbosity level (can be used multiple times) |
| `-q, --quiet` | Quiet mode (only errors and results) |
| `-o, --output FILE` | Write report to FILE |
| `-g, --graph FILE` | Generate dependency graph in DOT/PNG/SVG/PDF format |
| `-e, --exclude DIR` | Exclude directory from analysis (repeatable) |
| `-f, --exclude-file FILE` | Exclude file from analysis (repeatable) |
| `-d, --max-depth N` | Set maximum recursion depth (default: 100) |

### Verbosity Levels

Da'era supports different verbosity levels:

- **Level 0 (quiet)**: Only errors and final results
- **Level 1 (normal)**: Status updates and warnings
- **Level 2 (verbose)**: Detailed information about the analysis
- **Level 3 (debug)**: Full debugging information

Increase verbosity by using `-v` multiple times or using `-vv`.

### Examples

Analyze a project with normal output:
```bash
daera ~/projects/myproject
```

Generate a detailed report with visualization:
```bash
daera -v -o report.txt -g dependency_graph.png ~/projects/myproject
```

Exclude virtual environments and caches:
```bash
daera -e venv -e __pycache__ -e .venv ~/projects/myproject
```

Maximum verbosity for debugging:
```bash
daera -vv ~/projects/myproject
```

Generate different graph formats:
```bash
# DOT format (GraphViz source)
daera -g deps.dot ~/projects/myproject

# PNG image
daera -g deps.png ~/projects/myproject

# SVG vector graphics
daera -g deps.svg ~/projects/myproject

# PDF document
daera -g deps.pdf ~/projects/myproject
```

## Output

Da'era provides different outputs:

1. **Terminal output** with color-coded information based on verbosity level
2. **Text report** listing all identified circular dependencies when using `-o` option
3. **Visual dependency graph** showing module relationships when using `-g` option

Example terminal output:
```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  (Da'era) - Circular Dependency Analyzer v1.0       ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛

[STATUS] Analyzing project directory: /home/user/projects/myproject
[STATUS] Building dependency graph...
[INFO] Found 57 Python files
[STATUS] Detecting circular dependencies...
Found 2 circular dependencies:

Cycle 1: app.models.user -> app.services.auth -> app.models.user
Cycle 2: app.api.views -> app.api.serializers -> app.api.views

[ERROR] Found 2 circular dependencies.
```

## How It Works

Da'era analyzes your Python project through several steps:

1. **File Discovery**: Finds all Python files in the specified directory
2. **AST Parsing**: Uses Python's Abstract Syntax Tree to extract import statements
3. **Graph Building**: Constructs a directed graph of module dependencies
4. **Cycle Detection**: Implements a depth-first search algorithm to find cycles
5. **Visualization**: Optionally generates a visual representation using GraphViz

## Requirements

- Bash shell
- Python 3.6+ with standard library modules: ast, sys, os, re
- Common Unix utilities: find, grep, awk, sed
- GraphViz (optional, for generating visual graphs)

## License

Licensed under MIT License. See LICENSE file for details.

