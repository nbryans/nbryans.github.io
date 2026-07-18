# nbryans.github.io

Personal site/blog. Jekyll + minima theme, hosted on GitHub Pages.

## Local dev

Requires Ruby 3.3.0 (see `.ruby-version`) and Bundler.

```sh
bundle install
bundle exec jekyll serve
```

Serves at `http://localhost:4000` with `--watch` on by default (auto-rebuilds on file changes; new/renamed files under `_posts/` need a server restart to be picked up). Drafts and future-dated posts are excluded unless you pass `--drafts` / `--future`.

Build without serving (output in `_site/`):

```sh
bundle exec jekyll build
```

Post filenames must match `YYYY-MM-DD-title.md` or Jekyll silently drops them from the build — no error, no warning.

## New machine setup (macOS)

```sh
xcode-select --install        # native ext toolchain, needed for nokogiri et al.

brew install rbenv ruby-build
rbenv init

rbenv install 3.3.0            # matches .ruby-version

git clone git@github.com:nbryans/nbryans.github.io.git
cd nbryans.github.io
```

**Before running `bundle install`, verify rbenv is actually wired into `PATH`:**

```sh
which ruby     # must print .../.rbenv/shims/ruby — NOT /usr/bin/ruby or /System/...
ruby -v        # must print 3.3.0
```

If `which ruby` points anywhere under `/usr/bin` or `/System/Library`, the shims aren't loaded — `rbenv init` was appended to the wrong rc file (or a stale shell session is still active). Fix that before proceeding; installing gems against system Ruby will fail or silently land in the wrong place. Once `ruby -v` reports 3.3.0:

```sh
bundle install
```

Then follow **Local dev** above. If `bundle install` fails building `nokogiri`/`ffi`, confirm Xcode CLT finished installing (`xcode-select -p`) and retry.

## Enhancements

Ideas for future improvement, not yet scheduled:

1. Add pagination (`paginate` / `paginate_path` in `_config.yml`) once the number of posts on the home page grows large enough to matter.
2. Move to the [Chirpy theme](https://chirpy.cotes.page/posts/text-and-typography/) for an improved UI/reading experience.
