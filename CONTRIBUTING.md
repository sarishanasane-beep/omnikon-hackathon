# Contributing to PHOENIX

Thank you for considering contributing to this project! This document provides guidelines for contributing to our hackathon project.

## Code of Conduct

This project adheres to a Code of Conduct that all contributors are expected to follow. Please read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) before contributing.

## How Can I Contribute?

### Reporting Bugs

If you find a bug, please create an issue with:
- Clear, descriptive title
- Detailed steps to reproduce
- Expected vs actual behavior
- Screenshots (if applicable)
- Environment details (OS, browser, versions)

### Suggesting Features

Feature suggestions are welcome! Please:
- Check existing issues first to avoid duplicates
- Provide clear use case and rationale
- Describe proposed solution or implementation ideas

### Pull Requests

We welcome pull requests! Here's the process:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
4. **Test thoroughly**
5. **Commit with clear messages**
   ```bash
   git commit -m "Add: brief description of changes"
   ```
6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```
7. **Open a Pull Request**

## Development Setup

### Prerequisites

**Hardware:**
- ESP32 development boards
- SX1278 LoRa modules
- Sensors: MAX30102, MPU6050, BMP280, NEO-6M GPS
- 0.96" OLED display

**Software:**
```bash
Python >= 3.8
PlatformIO or Arduino IDE
Git
```

### Installation

```bash
# Clone your fork
git clone https://github.com/your-username/omnikon-hackathon.git
cd omnikon-hackathon

# Backend dependencies
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install fastapi uvicorn

# Firmware setup
# Open firmware/ in PlatformIO or Arduino IDE
# Install required libraries (RadioLib, MAX30102, MPU6050, etc.)
```

### Running Locally

```bash
# Start backend API
cd backend
uvicorn main:app --reload

# Upload firmware to ESP32 devices via PlatformIO/Arduino IDE

# Open dashboard
cd dashboard
python -m http.server 8080
```

## Coding Standards

### Style Guide

- Use consistent indentation (2 or 4 spaces)
- Follow existing code style
- Write meaningful variable and function names
- Keep functions focused and modular

### Commit Message Guidelines

Use clear, descriptive commit messages:

- `Add: new feature or file`
- `Fix: bug fix`
- `Update: changes to existing functionality`
- `Refactor: code restructuring without behavior change`
- `Docs: documentation changes`
- `Test: adding or updating tests`

### Code Review Process

1. All PRs require review before merging
2. Address reviewer feedback promptly
3. Keep PRs focused and reasonably sized
4. Update documentation if needed

## Testing

- Write tests for new features
- Ensure all tests pass before submitting PR
- Include both unit and integration tests when applicable

```bash
# Run tests
npm test
```

## Security

- Never commit API keys, passwords, Wi-Fi credentials, or sensitive data
- Use environment variables or config files for secrets (excluded via `.gitignore`)
- Test LoRa encryption if implementing security features
- Report security vulnerabilities privately (see [SECURITY.md](SECURITY.md))

## Attribution

All contributors will be credited in:
- README.md Team Members section
- Git commit history
- Release notes

## Hackathon Timeline

This is an Omnikon Hackathon 2026 project with specific deadlines. Please keep timing in mind when contributing and prioritize features critical for demo readiness.

## Questions?

Feel free to:
- Open an issue for discussion
- Contact team members:
  - **Sarish Anasane**: sarishanasane@gmail.com | +91 9511231195
  - **Arya Ghate**: aryaghate11@gmail.com | +91 7385749059

## License

By contributing, you agree that your contributions will be licensed under the same license as the project (MIT License).

---

Thank you for contributing to PHOENIX! Together, we're building technology that saves lives.
