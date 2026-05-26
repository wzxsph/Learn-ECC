# Learn-ECC Contributing Guide

Welcome to contributing to Learn-ECC! This guide explains how to add new content, improve existing content, or submit changes.

## Contribution Types

### 1. Add New Content

Types of content you can contribute:
- New chapters or modules
- New exercises
- New practical projects
- Cheatsheet additions
- Translation improvements

### 2. Improve Existing Content

- Fix errors
- Improve explanation clarity
- Add more examples
- Improve exercise answers
- Update outdated information

### 3. Report Issues

- Typographical or grammatical errors
- Non-working code examples
- Broken links
- Missing content

## Content Format Standards

### Markdown Format

All content must use Markdown format, containing the following sections:

```markdown
# Title

## Learning Objectives

After completing this section, you will be able to:
- Objective 1
- Objective 2

---

## Main Content

### Subheading

Content...

---

## Exercises

### Exercise 1

Task description...

### Verification Methods

1. Verification step 1
2. Verification step 2

---

## Next Steps

- Next chapter: [Link](./next.md)
- Return: [Table of Contents](../README.md)
```

### File Naming

- Use Chinese naming
- Use hyphens `-` to separate words
- Chapter file naming: `01-chapter-name.md`, `02-chapter-name.md`
- Exercise file naming: `exercise-name.md`

### Link Conventions

- Textbook links: `./Reference-Docs/file-name.md`
- Course internal links: `./chapter-name.md`
- Relative paths: calculated from current file location

### Code Examples

```javascript
// Code language annotation
function example() {
  return "Hello ECC"
}
```

```bash
# Command line example
node tests/run-all.js
```

### Table Format

| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Content 1 | Content 2 | Content 3 |

## Submitting Changes

### Steps

1. Fork the repository
2. Create a branch: `git checkout -b feature/new-content-description`
3. Make changes
4. Commit: `git commit -m "feat: add new content"`
5. Push to remote: `git push origin feature/new-content-description`
6. Create a Pull Request

### Commit Message Convention

```
<type>: <description>

<optional body>
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation update
- `test`: Test update
- `refactor`: Refactoring

## Quality Checks

Before submitting, please check:

- [ ] Markdown format is correct
- [ ] All links are valid
- [ ] Code examples are runnable
- [ ] Exercises have verification methods
- [ ] No typos in Chinese
- [ ] Conforms to format standards

## Feedback

If you find issues or suggestions, please create an Issue including:
- Issue description
- File location
- Suggested improvement

---

Thank you for your contribution!