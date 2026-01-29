# GitHub Actions Build Fix

## Problem

GitHub Actions was failing with:
```
Error: The process '/opt/hostedtoolcache/Ruby/3.1.3/x64/bin/gem' failed with exit code 1
```

During the "Installing Bundler" step.

## Root Cause

The `Gemfile.lock` file was generated locally with Bundler 2.7.1, which has compatibility issues with:
1. The Ruby setup action in GitHub Actions
2. Ruby 3.1.3's default Bundler version
3. The eventmachine gem compilation on different platforms

## Solution

### 1. Updated GitHub Actions Workflow

**Changed:**
- Old: `uses: ruby/setup-ruby@ee2113536afb7f793eed4ce60e8d3b26db912da4` (specific commit)
- New: `uses: ruby/setup-ruby@v1` (latest version)
- Incremented `cache-version: 1` to bust old cache

**Why:** The latest ruby/setup-ruby handles bundler versions better and works more reliably across platforms.

### 2. Removed Gemfile.lock from Git

Added to `.gitignore`:
```
# Let GitHub Actions generate its own Gemfile.lock
Gemfile.lock
```

**Why:** 
- Different platforms (macOS, Linux) have different gem requirements
- GitHub Actions will generate its own platform-specific lockfile
- Avoids eventmachine and other native extension conflicts
- Standard practice for Jekyll/GitHub Pages projects

### 3. How It Works Now

1. **Local Development:** Uses installed gems directly (no Gemfile.lock)
2. **GitHub Actions:** Generates its own Gemfile.lock during build
3. **Both:** Use the same `Gemfile` source, ensuring consistency

## Testing

The next push to `main` branch will:
1. Checkout code (no Gemfile.lock)
2. Run `bundle install` (generates fresh lockfile)
3. Cache the gems for faster future builds
4. Build Jekyll site
5. Deploy to GitHub Pages

## Local Development Impact

**No change needed!** Local development continues to work:
```bash
script/server    # Works as before
script/cibuild   # Works as before
```

The only difference is no Gemfile.lock is committed to git.

## Benefits

✅ **No more platform conflicts** - Each environment manages its own dependencies
✅ **Simpler maintenance** - No need to sync Gemfile.lock between macOS and Linux  
✅ **Faster iterations** - No lockfile conflicts during git operations
✅ **Standard practice** - Matches GitHub Pages documentation recommendations

## Alternative Approaches Considered

1. **Pin Bundler version in workflow** - Would require manual updates
2. **Commit separate lockfiles** - Too complex, hard to maintain
3. **Use Docker locally** - Overkill for this project
4. **Keep lockfile, update gems** - Still has platform incompatibilities

Removing the lockfile is the cleanest solution for Jekyll sites deployed to GitHub Pages.

## References

- [GitHub Pages Jekyll documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll)
- [ruby/setup-ruby action](https://github.com/ruby/setup-ruby)
- [Bundler best practices](https://bundler.io/guides/rationale.html)
