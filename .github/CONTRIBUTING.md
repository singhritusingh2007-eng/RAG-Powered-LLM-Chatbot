# Contributing to RAG-Powered LLM Chatbot

We welcome contributions! This document explains our contribution process.

## Getting Started

1. **Fork the repository**
   ```bash
   git clone https://github.com/YOUR-USERNAME/RAG-Powered-LLM-Chatbot.git
   ```

2. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Follow PEP 8 style guidelines
   - Add docstrings to functions
   - Write tests for new features

4. **Push and create a PR**
   ```bash
   git push origin feature/your-feature-name
   ```

## Types of Contributions

### Bug Reports
- Use GitHub Issues
- Include Python version and OS
- Provide minimal reproducible example

### Feature Requests
- Describe the use case
- Explain expected behavior
- Optional: provide implementation suggestion

### Code Improvements
- Performance optimizations
- Code quality improvements
- Documentation fixes
- Test coverage improvements

## Code Style

```python
# Good
def load_documents(file_path: str) -> list[str]:
    """Load documents from file path."""
    with open(file_path, 'r') as f:
        return f.readlines()

# Bad
def ld(x):
    f = open(x)
    return f.readlines()
```

## Testing

```bash
# Run tests before submitting PR
python -m pytest tests/
```

## Commit Messages

```
[Type] Brief description (50 chars max)

Optional longer explanation with context.
Explain what and why, not how.

Fixes #123
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`

## Pull Request Process

1. Update README.md with relevant changes
2. Add tests for new features
3. Ensure all tests pass
4. Request review from maintainers
5. Address feedback and iterate

## Code of Conduct

- Be respectful and inclusive
- No harassment or discrimination
- Constructive feedback only

---

**Thank you for contributing!** 🙌