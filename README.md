# Development Environment Configuration

Documentation of the development environment, software configuration, and editor settings used for reproducible research and programming.

**Last updated:** 2026-10-03  
**Operating system:** macOS  
**Architecture:** Apple Silicon

---

## 1. Visual Studio Code

### General settings

The main VS Code configuration is stored in:

```text
~/Library/Application Support/Code/User/settings.json
```

Current relevant configuration:

```json
{
    "workbench.colorTheme": "Julia (Monokai Vibrant)",
    "editor.fontSize": 11,
    "editor.inlineSuggest.enabled": false,

    "files.associations": {
        "*.rmd": "markdown"
    },

    "r.lsp.diagnostics": false,

    "r.consolePath": "/Users/mfig/.local/pipx/venvs/radian/bin/radian",
    "r.consoleArgs": [
        "--no-save",
        "--no-restore"
    ],

    "r.sessionWatcher": true,
    "r.alwaysUseActiveTerminal": true,
    "r.bracketedPaste": true,

    "r.plot.backend": "auto",

    "r.session.viewers.viewColumn": {
        "helpPanel": "Beside",
        "plot": "Active"
    },

    "python.terminal.activateEnvironment": false,

    "julia.symbolCacheDownload": true,
    "julia.enableTelemetry": true,

    "latex-workshop.docker.enabled": false,

    "security.workspace.trust.banner": "always",

    "julia.usePlotPane": true,
    "julia.focusPlotNavigator": true,

    "git.autofetch": true
}
```

### VS Code extensions

The main extensions used for scientific computing are:

| Extension | Purpose |
|---|---|
| `REditorSupport.r` | R language support |
| `Julia` | Julia language support |
| `LaTeX Workshop` | LaTeX editing and compilation |
| Git-related extensions | Version control |
| Python extension | Python development |

> The exact extension versions should be recorded periodically because VS Code extensions can change their behaviour between releases.

To obtain the installed extensions:

```bash
code --list-extensions --show-versions
```

Save the output with:

```bash
code --list-extensions --show-versions > vscode-extensions.txt
```

---

# 2. R

### R installation

Check the active R installation with:

```bash
which R
R --version
```

Inside R:

```r
R.home()
R.version.string
Sys.which("R")
Sys.getenv("R_HOME")
```

The R installation used by the development environment should be documented here.

Example:

```text
R version: <VERSION>
R location: <PATH>
Architecture: arm64
```

---

# 3. Radian

Radian is used as the R console inside VS Code.

### Executable

```text
/Users/mfig/.local/pipx/venvs/radian/bin/radian
```

Check the installation with:

```bash
which radian
radian --version
```

The Radian executable can also be started directly with:

```bash
/Users/mfig/.local/pipx/venvs/radian/bin/radian
```

### VS Code configuration

Radian is selected through:

```json
"r.consolePath": "/Users/mfig/.local/pipx/venvs/radian/bin/radian"
```

with:

```json
"r.consoleArgs": [
    "--no-save",
    "--no-restore"
]
```

### Radian diagnostic information

Inside Radian/R:

```r
R.home()
R.version.string
Sys.which("R")
Sys.getenv("R_HOME")
Sys.getpid()
```

The PID is useful for determining whether the R session has been restarted unexpectedly.

---

# 4. R session management in VS Code

The following settings control the interaction between VS Code and the R session:

```json
"r.sessionWatcher": true,
"r.alwaysUseActiveTerminal": true,
"r.bracketedPaste": true
```

### `r.sessionWatcher`

```json
"r.sessionWatcher": true
```

Allows the R extension to monitor the active R session.

### `r.alwaysUseActiveTerminal`

```json
"r.alwaysUseActiveTerminal": true
```

Ensures that code is sent to the active terminal rather than allowing VS Code to select another terminal.

This is particularly important when several R/Radian terminals are open.

### `r.bracketedPaste`

```json
"r.bracketedPaste": true
```

Used for reliable transmission of multi-line R code to Radian.

---

# 5. Diagnosing unexpected R session resets

If objects unexpectedly disappear from the R workspace, first check the process ID:

```r
Sys.getpid()
```

Then check the workspace:

```r
ls()
```

and:

```r
ls(envir = .GlobalEnv)
```

For a specific object:

```r
exists("object_name")
```

If the PID changes unexpectedly:

```text
PID before
    ↓
R session terminates/restarts
    ↓
PID after
```

then the problem is likely related to the R/Radian/VS Code session rather than the R code itself.

If the PID remains unchanged but objects disappear, investigate workspace manipulation or the possibility that code is being executed in another environment/session.

---

# 6. Python

Python environments should be documented separately from VS Code.

Check:

```bash
which python
python --version
```

and:

```bash
which pip
pip --version
```

If using `pipx`:

```bash
pipx list
```

Current relevant VS Code setting:

```json
"python.terminal.activateEnvironment": false
```

This prevents automatic activation of a Python environment when opening a terminal.

---

# 7. Julia

Julia is used through the VS Code Julia extension.

Relevant settings:

```json
"julia.symbolCacheDownload": true,
"julia.enableTelemetry": true,
"julia.usePlotPane": true,
"julia.focusPlotNavigator": true
```

Check the Julia installation:

```bash
which julia
julia --version
```

Inside Julia:

```julia
VERSION
Sys.BINDIR
```

The active Julia environment can be checked with:

```julia
Base.active_project()
```

---

# 8. LaTeX

LaTeX compilation is managed through the VS Code **LaTeX Workshop** extension.

Current setting:

```json
"latex-workshop.docker.enabled": false
```

Docker is therefore not used by LaTeX Workshop.

Check the local LaTeX installation with:

```bash
which pdflatex
pdflatex --version
```

Depending on the installation, also check:

```bash
which xelatex
xelatex --version
```

and:

```bash
which lualatex
lualatex --version
```

---

# 9. Git

Git is used for version control and GitHub repositories.

Check:

```bash
git --version
```

Git configuration:

```bash
git config --global --list
```

VS Code setting:

```json
"git.autofetch": true
```

This enables automatic fetching of remote Git information.

---

# 10. macOS shell

Check the active shell:

```bash
echo $SHELL
```

and:

```bash
echo $PATH
```

Useful information to record:

```text
Shell:
Terminal:
Architecture:
Package manager:
```

For Homebrew:

```bash
brew --version
brew --prefix
```

---

# 11. pipx

Python command-line applications installed through `pipx` can be inspected using:

```bash
pipx list
```

The current Radian installation is managed through:

```text
~/.local/pipx/venvs/radian/
```

Check:

```bash
ls ~/.local/pipx/venvs/radian/
```

---

# 12. Reproducibility

For R projects, package versions can be exported with:

```r
sessionInfo()
```

For a more complete record:

```r
installed.packages()[, c("Package", "Version")]
```

A project-specific package environment should preferably be managed with `renv`.

Initialize:

```r
renv::init()
```

Snapshot:

```r
renv::snapshot()
```

Restore:

```r
renv::restore()
```

The resulting:

```text
renv.lock
```

should normally be committed to Git.

---

# 13. Useful diagnostic commands

### VS Code

```bash
code --version
code --list-extensions --show-versions
```

### R

```bash
which R
R --version
```

### Radian

```bash
which radian
radian --version
```

### Python

```bash
which python
python --version
```

### Julia

```bash
which julia
julia --version
```

### Git

```bash
git --version
```

### LaTeX

```bash
which pdflatex
pdflatex --version
```

### Homebrew

```bash
brew --version
brew --prefix
```

### macOS

```bash
sw_vers
uname -m
```

---

# 14. Environment snapshot

The following commands can be used to create a snapshot of the current environment:

```bash
mkdir -p environment

code --version > environment/vscode-version.txt

code --list-extensions --show-versions \
    > environment/vscode-extensions.txt

R --version \
    > environment/r-version.txt

radian --version \
    > environment/radian-version.txt

python --version \
    > environment/python-version.txt

julia --version \
    > environment/julia-version.txt

git --version \
    > environment/git-version.txt

pdflatex --version \
    > environment/latex-version.txt

sw_vers \
    > environment/macos-version.txt

uname -m \
    >> environment/macos-version.txt
```

This provides a lightweight record of the computational environment without committing the complete VS Code installation or system directories.

---

# 15. Files that should be version controlled

A useful repository structure is:

```text
environment/
├── README.md
├── vscode-settings.json
├── vscode-extensions.txt
├── r-version.txt
├── radian-version.txt
├── python-version.txt
├── julia-version.txt
├── latex-version.txt
├── git-version.txt
└── macos-version.txt
```

For R projects using `renv`:

```text
renv.lock
```

should also be committed.

---

# 16. Important security note

Do **not** commit:

```text
.env
.env.*
*.pem
*.key
credentials.json
secrets.json
```

or files containing:

- API keys
- access tokens
- passwords
- SSH private keys
- GitHub tokens
- cloud credentials
- personal authentication information

A VS Code `settings.json` is generally safe to document, but inspect it for credentials before committing it to a public repository.

---

# 17. Current environment summary

| Component | Configuration |
|---|---|
| Operating system | macOS |
| Architecture | Apple Silicon |
| Editor | Visual Studio Code |
| R console | Radian |
| R console path | `~/.local/pipx/venvs/radian/bin/radian` |
| R session watcher | Enabled |
| Active terminal enforcement | Enabled |
| Bracketed paste | Enabled |
| R diagnostics | Disabled |
| Python automatic environment activation | Disabled |
| Julia plot pane | Enabled |
| Julia symbol cache | Enabled |
| LaTeX Docker | Disabled |
| Git autofetch | Enabled |

---

## Maintenance

This document should be updated whenever one of the following changes:

- macOS version
- VS Code version
- VS Code extensions
- R version
- Radian version
- Python version
- Julia version
- LaTeX distribution
- Git configuration
- major VS Code settings
- project-level package environments

The goal is to maintain a reproducible record of the computational environment used for research and software development.
