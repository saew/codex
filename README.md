# 🚀 Codex — Lightweight Terminal Coding Agent for Developers & Automation

[![Release](https://img.shields.io/github/v/release/saew/codex?style=for-the-badge&color=2b9348)](https://github.com/saew/codex/releases)

![Terminal Workflow](https://images.unsplash.com/photo-1517336714731-489689fd1ca8?auto=format&fit=crop&w=1600&q=60)

A compact coding agent that runs in your terminal. Codex helps you scaffold code, run tests, manage snippets, and automate small developer tasks without leaving the shell. It focuses on speed, low memory use, and predictable behavior.

Table of contents
- About
- Features
- Quickstart
- Install (release download + execute)
- Basic usage
- Commands and options
- Configuration
- Examples
- Integration tips
- Development
- Contributing
- License

About
Codex runs in the terminal. It reads your project files and acts on commands you give. Use it for scaffolding, refactoring helpers, snippet storage, and small automation tasks. It targets developers who prefer the shell and want a compact agent they can run locally.

Features
- Small binary with no heavy runtime dependency.
- Interactive shell mode and single-command mode.
- Snippet manager with quick search.
- Test runner hooks that integrate with common test frameworks.
- File-aware commands (edit, refactor, summarize).
- Local configuration and plugin interface.
- Cross-platform builds: Linux, macOS, Windows.

Quickstart
1. Visit the Releases page to pick a build and assets:
   https://github.com/saew/codex/releases
2. Download the appropriate release asset for your OS.
3. Extract the archive and run the binary file you downloaded.

Install (release download + execute)
The Releases page hosts compiled binaries and archives. Download the file for your OS from the release you want and run the included executable.

Example steps for Linux x86_64 (adjust for your OS):

```bash
# download the binary archive from Releases
curl -L -o codex-linux-amd64.tar.gz \
  https://github.com/saew/codex/releases/download/v1.2.0/codex-linux-amd64.tar.gz

# extract
tar -xzf codex-linux-amd64.tar.gz

# make executable
chmod +x codex

# move to a location on your PATH
sudo mv codex /usr/local/bin/

# verify
codex --version
```

If the download link or file name does not match, visit the Releases page and pick the correct file:
https://github.com/saew/codex/releases

Windows (PowerShell):

```powershell
# download a release asset
Invoke-WebRequest -Uri "https://github.com/saew/codex/releases/download/v1.2.0/codex-windows-amd64.zip" -OutFile "codex.zip"

# extract and run
Expand-Archive codex.zip -DestinationPath .\codex
.\codex\codex.exe --version
```

macOS (Homebrew-style install example):

```bash
# if you prefer a local copy
curl -L -o codex-macos.tar.gz \
  https://github.com/saew/codex/releases/download/v1.2.0/codex-macos-amd64.tar.gz

tar -xzf codex-macos.tar.gz
chmod +x codex
mv codex /usr/local/bin/
codex --help
```

Basic usage
Run the agent in single-command mode:

```bash
# scaffold a new REST endpoint
codex scaffold endpoint --name CreateUser --language go --path ./api/users
```

Run interactive shell mode:

```bash
codex shell
```

In shell mode you can run commands, search snippets, or ask for a summary of a file.

Commands and options
Codex exposes a small set of commands that cover most workflows:

- codex help
  - Show the top-level help.
- codex version
  - Print the build version and platform.
- codex shell
  - Start interactive mode.
- codex scaffold [type]
  - Create boilerplate for endpoints, modules, tests.
  - Options: --name, --language, --path
- codex snippet [add|get|search|list|rm]
  - Manage reusable snippets.
- codex test [run|watch]
  - Hook into your test runner. Use --pattern to limit tests.
- codex lint [fix]
  - Run linters that match your project language.
- codex refactor --file <path> --action <rename|extract|inline>
  - Perform file-aware refactors.

Flags common to many commands:
- --config <path> : point to a different config file.
- --yes : skip prompts and run with defaults.
- --output <path> : write command output to a file.

Configuration
Codex uses a small YAML config at the root of your project: .codex.yml

Example .codex.yml:

```yaml
language: go
test:
  runner: go test
snippets:
  path: .codex/snippets
plugins:
  - name: shell-helper
    path: ./plugins/shell-helper
```

- language: default language for scaffold commands.
- test.runner: the command to run tests.
- snippets.path: location to store snippet files.
- plugins: local plugin entries.

Snippets
Store small code blocks in the snippet store. Use tags to find items.

Add a snippet:

```bash
codex snippet add --name "jwt-middleware" --tag "go,auth" --file ./examples/jwt-mw.go
```

Search snippets:

```bash
codex snippet search jwt
```

Examples
Scaffold a handler and a test for Node.js:

```bash
codex scaffold endpoint --name CreateUser --language node --path ./services/users
codex test run --pattern services/users
```

Extract a function from a large file:

```bash
codex refactor --file server/handler.go --action extract --range "120:200" --name ExtractedFunc
```

Run Codex to summarize a file:

```bash
codex summarize server/handler.go
```

This prints a short summary of top-level functions and their responsibilities.

Integration tips
- Use shell aliases to speed up your flow:
  - alias cx="codex"
- Combine Codex with your editor:
  - Bind a key in Vim/Neovim to call codex for refactor commands.
- Add codex commands to CI for automated scaffolding checks.

Development
The repository contains a small CLI core and plugin system. The structure looks like:

- cmd/        CLI entrypoints
- core/       core logic and file analysis
- plugins/    plugin interface and examples
- assets/     templates for scaffolding
- docs/       additional docs and examples

Build locally:

```bash
# Go-based build example
go build -o codex ./cmd/codex
./codex --version
```

Testing:

```bash
go test ./...
```

Plugin system
Plugins live in the plugins directory and expose a small RPC interface. A plugin can implement commands, add templates, or hook test runners. Use the plugin template in plugins/template to start.

Securing execution
Codex runs local code transformations. Keep your plugin list small and audit third-party plugins before adding them to your projects. Use the config to restrict plugins by path.

Troubleshooting
- If a command fails, run with --debug to get verbose logs.
- If you cannot find the release file you need, visit the Releases page:
  https://github.com/saew/codex/releases
- If a plugin causes errors, disable it in .codex.yml and restart Codex.

Roadmap
- Add language server protocol integration for richer code actions.
- Expand plugin registry with vetted community plugins.
- Add native package managers for common languages for quick scaffold templates.

Contributing
Contributions help the project grow. Follow these steps:

1. Fork the repo.
2. Create a topic branch.
3. Implement your change.
4. Add tests.
5. Open a pull request with a clear description.

Follow the code style in the repo. Keep commits focused and use present-tense in commit messages.

Code of conduct
Be respectful. Keep discussions technical and constructive.

Changelog and releases
Find compiled builds and release notes on the Releases page:
https://github.com/saew/codex/releases

If you need a specific binary, download the file for your platform from the selected release and execute it on your machine.

Community
- Open issues for bugs and feature requests.
- Use PRs for code contributions.
- Discuss design decisions in issues and PR threads.

License
Codex uses the MIT License. See LICENSE for details.