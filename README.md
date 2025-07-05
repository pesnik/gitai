# GitAI 🤖

AI-powered commit message generator using local LLMs via Ollama. Generate meaningful, conventional commit messages automatically from your staged changes.

## Features

- 🚀 **Local AI**: Uses Ollama for privacy and offline capability
- 📝 **Conventional Commits**: Follows industry-standard commit message format
- ⚡ **Fast**: Optimized for quick generation with model caching
- 🔧 **Flexible**: Works as standalone CLI or integrated git hooks
- 🎯 **Smart**: Analyzes actual code changes, not just file names
- ⚙️ **Configurable**: Customize models, prompts, and behavior

## Installation

### Prerequisites

- [Ollama](https://ollama.ai) installed and running
- Git repository
- Go 1.21+ (for building from source)

### Install GitAI

```bash
# Using go install
go install github.com/pesnik/gitai@latest

# Or download from releases
curl -L https://github.com/pesnik/gitai/releases/latest/download/gitai-linux-amd64 -o gitai
chmod +x gitai
sudo mv gitai /usr/local/bin/
```

### Setup Ollama Model

```bash
# Download recommended model
ollama pull codellama:7b

# Or use a smaller model for faster generation
ollama pull llama3:8b
```

## Quick Start

### Standalone Usage

```bash
# Stage your changes
git add .

# Generate commit message
gitai generate

# Use the generated message
git commit -m "$(gitai generate)"
```

### Git Hook Integration

```bash
# Install git hooks in current repository
gitai install

# Now git commit will automatically generate messages
git add .
git commit  # Opens editor with AI-generated message
```

## Usage

### Commands

```bash
gitai generate          # Generate commit message for staged changes
gitai install           # Install git hooks in current repo
gitai uninstall         # Remove git hooks
gitai config            # Show current configuration
gitai config set <key> <value>  # Set configuration option
gitai version           # Show version information
```

### Configuration

```bash
# Set Ollama model
gitai config set model codellama:7b

# Set Ollama host (if not default)
gitai config set host http://localhost:11434

# Customize prompt template
gitai config set prompt "Custom prompt template..."

# Set max diff size (in lines)
gitai config set max-diff-lines 500
```

### Configuration File

GitAI stores configuration in `~/.gitai/config.yaml`:

```yaml
model: "codellama:7b"
host: "http://localhost:11434"
timeout: 30
max_diff_lines: 500
prompt_template: |
  Analyze this git diff and generate a concise commit message following conventional commits format:
  
  {diff}
  
  Rules:
  - Start with type: feat, fix, docs, style, refactor, test, chore
  - Include scope in parentheses if applicable
  - Use present tense, imperative mood
  - Keep subject line under 72 characters
  - No period at the end of subject line
  
  Generate only the commit message, no explanation.
```

## Examples

### Generated Messages

```bash
# Adding a new feature
feat(auth): add OAuth2 login integration

# Fixing a bug
fix(parser): handle null values in JSON response

# Refactoring code
refactor(utils): extract string validation to separate function

# Updating documentation
docs(readme): add installation instructions for Windows

# Adding tests
test(auth): add unit tests for login validation
```

### Workflow Integration

```bash
# Your typical workflow
git add src/auth.go
git commit
# Editor opens with: "feat(auth): add JWT token validation"
# Edit if needed, save and close
```

## Advanced Usage

### Custom Prompts

Create custom prompt templates for specific project needs:

```bash
gitai config set prompt "$(cat <<EOF
You are a senior developer writing commit messages for a React application.
Analyze this diff and create a commit message that:
- Follows conventional commits
- Mentions component names when relevant
- Includes performance impact if applicable

Diff:
{diff}

Generate only the commit message:
EOF
)"
```

### Multiple Models

Switch between models for different scenarios:

```bash
# Fast model for quick commits
gitai config set model llama3:8b

# Detailed model for complex changes
gitai config set model codellama:13b
```

### Integration with Git Aliases

Add to your `.gitconfig`:

```ini
[alias]
    ai = !gitai generate
    aic = !git add . && git commit -m "$(gitai generate)"
```

## Troubleshooting

### Common Issues

**Ollama not responding**
```bash
# Check if Ollama is running
ollama list

# Start Ollama if needed
ollama serve
```

**Model not found**
```bash
# Pull the model
ollama pull codellama:7b

# List available models
ollama list
```

**Large diffs timing out**
```bash
# Reduce max diff size
gitai config set max-diff-lines 200

# Or use a smaller/faster model
gitai config set model llama3:8b
```

**Generated messages are poor quality**
```bash
# Try a different model
gitai config set model codellama:13b

# Or customize the prompt
gitai config set prompt "Your custom prompt here..."
```

## Development

### Building from Source

```bash
git clone https://github.com/pesnik/gitai.git
cd gitai
go build -o gitai ./cmd/gitai
```

### Running Tests

```bash
go test ./...
```

### Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Acknowledgments

- [Ollama](https://ollama.ai) for providing local LLM capabilities
- [Conventional Commits](https://www.conventionalcommits.org/) for the commit message standard
- The Go community for excellent tooling and libraries

---

**Made with ❤️ for developers who want better commit messages without sacrificing privacy**
