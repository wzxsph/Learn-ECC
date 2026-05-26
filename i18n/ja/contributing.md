# Learn-ECC Contribution Guide

Welcome to contributing to Learn-ECC! This guide explains how to add new content, improve existing content, or submit changes.

## Contribution Types

### 1. Add New Content

Types of content you can contribute:
- New chapters or modules
- New exercises
- New practical projects
- Cheat sheet additions
- Translation improvements

### 2. Improve Existing Content

- Fix errors
- Improve explanation clarity
- Add more examples
- Improve exercise answers
- Update outdated information

### 3. Report Issues

- Typos or grammatical errors
- Code examples that don't work
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

### Subtitle

Content...

---

## Exercises

### Exercise 1

Task description...

### Verification Method

1. Verification step 1
2. Verification step 2

---

## Next Steps

- Next chapter: [Link](./next.md)
- Return: [Table of contents](../README.md)
```

### File Naming

- Use Chinese naming
- Use hyphen `-` to separate words
- Chapter file naming: `01-ChapterName.md`, `02-ChapterName.md`
- Exercise file naming: `Exercise-Name.md`

### Link Standards

- Reference links: `./参考ドキュメント/fileName.md`
- Course internal links: `./chapterName.md`
- Relative path: Calculated from current file location

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
|-------|-------|-------|
| Content 1 | Content 2 | Content 3 |

## Submitting Changes

### Steps

1. Fork the repository
2. Create branch: `git checkout -b feature/new-content-description`
3. Make changes
4. Commit: `git commit -m "feat: add new content"`
5. Push to remote: `git push origin feature/new-content-description`
6. Create Pull Request

### Commit Message Standards

```
<type>: <description>

<optional body>
```

Type types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation update
- `test`: Test update
- `refactor`: Refactoring

## Quality Check

Before submitting, please check:

- [ ] Markdown format is correct
- [ ] All links are valid
- [ ] Code examples are runnable
- [ ] Exercises have verification methods
- [ ] No typos in Chinese
- [ ] Meets format standards

## Issue Feedback

If you find problems or have suggestions, please create an Issue containing:
- Problem description
- File location
- Suggested improvement method

---

Thank you for your contribution!