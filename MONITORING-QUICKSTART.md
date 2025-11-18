# Crelly Slider - Monitoring Quick Start Guide

**For:** Code reviewers and maintainers
**Updated:** November 18, 2025

---

## Quick Reference

### Check for New Commits

```bash
# Fetch latest changes
git fetch origin nightly

# View new commits
git log --oneline origin/nightly ^HEAD

# See what files changed
git diff --name-status HEAD..origin/nightly
```

### Review Checklist

**Time Required:** ~50-70 minutes per cycle

#### 1. Initial Assessment (5 min)
- [ ] Review commit messages
- [ ] Identify changed files
- [ ] Assess change scope
- [ ] Determine priority

#### 2. Security Review (20-30 min)
- [ ] Check all `$_GET`, `$_POST`, `$_REQUEST` usage
- [ ] Verify output escaping (`echo`, `print`)
- [ ] Validate SQL queries use `$wpdb->prepare()`
- [ ] Check nonce verification
- [ ] Review file operations
- [ ] Verify capability checks

#### 3. Compatibility Review (15-20 min)
- [ ] Check for deprecated WordPress functions
- [ ] Verify PHP 7.4+ compatibility
- [ ] Test against WordPress 6.7
- [ ] Review database changes
- [ ] Check jQuery compatibility

#### 4. Code Quality Review (10-15 min)
- [ ] WordPress Coding Standards
- [ ] PHPDoc comments
- [ ] Code organization
- [ ] Performance considerations
- [ ] Error handling

#### 5. Documentation (5 min)
- [ ] Update claude.md with findings
- [ ] Log review in MONITORING.md
- [ ] Create issue tickets if needed

---

## Severity Levels

| Level | Response Time | Action Required |
|-------|---------------|-----------------|
| 🔴 **CRITICAL** | Same day | Immediate fix + notify |
| 🟠 **HIGH** | 24-48 hours | Priority fix |
| 🟡 **MEDIUM** | 1 week | Scheduled fix |
| 🟢 **LOW** | Best effort | Backlog |

---

## Security Issues to Watch For

### Critical
- ❌ Remote code execution
- ❌ SQL injection
- ❌ Authentication bypass
- ❌ Privilege escalation

### High
- ⚠️ Stored XSS
- ⚠️ CSRF vulnerabilities
- ⚠️ Insecure cryptography
- ⚠️ Data exposure

### Medium
- ⚡ Input validation issues
- ⚡ Reflected XSS
- ⚡ Information disclosure
- ⚡ Session management

### Low
- 📝 Code style issues
- 📝 Documentation gaps
- 📝 Minor optimizations

---

## Commit Template

```bash
git commit -m "<type>(<scope>): <subject>

<body - explain what and why>

Fixes: #<issue-number>
CVSS Score: <score> (if security issue)

Co-authored-by: Claude <code@claude.ai>
Co-authored-by: Ojārs Kapteinis <ojars@kapteinis.lv>"
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `security`: Security fix
- `docs`: Documentation
- `style`: Code style
- `refactor`: Refactoring
- `perf`: Performance
- `test`: Tests
- `chore`: Maintenance

---

## Review Cycle Workflow

```
1. Fetch nightly branch
   ↓
2. Check for new commits
   ↓
3. Review changed files
   ↓
4. Document findings
   ↓
5. Update claude.md
   ↓
6. Commit documentation
   ↓
7. Push to claude/* branch
   ↓
8. Create pull request
```

---

## Common Security Patterns

### ✅ Good
```php
// Proper input sanitization
$value = sanitize_text_field($_POST['value']);

// Proper output escaping
echo esc_html($user_content);

// Proper SQL query
$results = $wpdb->get_results($wpdb->prepare(
    'SELECT * FROM table WHERE id = %d', $id
));

// Proper capability check
if (current_user_can('manage_options')) {
    // sensitive operation
}

// Proper nonce verification
if (wp_verify_nonce($_POST['nonce'], 'action_name')) {
    // process form
}
```

### ❌ Bad
```php
// No sanitization
$value = $_POST['value'];

// No escaping
echo $user_content;

// No prepared statement
$results = $wpdb->get_results("SELECT * FROM table WHERE id = " . $_GET['id']);

// No capability check
// anyone can access sensitive operation

// No nonce verification
// CSRF vulnerability
```

---

## WordPress Function Reference

### Input Sanitization
- `sanitize_text_field()` - Single line text
- `sanitize_textarea_field()` - Multi-line text
- `sanitize_email()` - Email addresses
- `sanitize_url()` - URLs
- `sanitize_key()` - Alphanumeric keys
- `absint()` - Positive integers
- `intval()` - Any integer
- `floatval()` - Float values

### Output Escaping
- `esc_html()` - HTML content
- `esc_attr()` - HTML attributes
- `esc_url()` - URLs
- `esc_js()` - JavaScript strings
- `wp_kses_post()` - Post content (allows safe HTML)
- `wp_kses()` - Custom allowed HTML

### Security Functions
- `current_user_can()` - Check capabilities
- `wp_verify_nonce()` - Verify nonces
- `check_ajax_referer()` - Verify AJAX nonces
- `wp_create_nonce()` - Create nonces
- `wp_rand()` - Cryptographically secure random

---

## Contact & Escalation

### For Critical Issues
1. Create private security advisory on GitHub
2. Email: ojars@kapteinis.lv
3. Document in claude.md immediately
4. Mark as CRITICAL in commit message

### For Questions
- Review MONITORING.md for detailed procedures
- Check claude.md for historical context
- Refer to WordPress Security documentation

---

## Useful Commands

```bash
# View file history
git log --follow -- path/to/file.php

# See who changed what
git blame path/to/file.php

# Compare branches
git diff nightly..origin/nightly

# Search commit messages
git log --grep="security"

# Find when a function was added
git log -S"function_name" --source --all

# View changes in a commit
git show <commit-hash>

# Interactive staging
git add -p
```

---

## Resources

- **Full Documentation:** See MONITORING.md
- **Review Report:** See claude.md
- **WordPress Security:** https://wordpress.org/about/security/
- **OWASP Top 10:** https://owasp.org/www-project-top-ten/
- **WP Coding Standards:** https://developer.wordpress.org/coding-standards/

---

**Maintained By:**
Claude (code@claude.ai)
Ojārs Kapteinis (ojars@kapteinis.lv)

**Last Updated:** November 18, 2025
