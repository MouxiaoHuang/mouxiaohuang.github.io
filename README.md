# mouxiaohuang.com

Personal homepage for [Mouxiao Huang](https://mouxiaohuang.com), built with Jekyll and hosted on GitHub Pages.

## Local setup

The repository targets the same Ruby and Jekyll versions used by GitHub Pages. Install Ruby using the version in `.ruby-version`, then run:

```bash
gem install bundler
bundle install
```

The GitHub Pages dependency set does not support Ruby 4.x yet, so use the version pinned in `.ruby-version` rather than the newest system Ruby.

### macOS with rbenv

If `rbenv versions` shows the Ruby version from `.ruby-version`, but `ruby -v` still reports Homebrew Ruby 4.x, initialize rbenv at the end of `~/.zshrc`:

```zsh
eval "$(rbenv init - zsh)"
```

Restart the terminal (or run `exec zsh`), then verify and install the dependencies:

```zsh
ruby -v
# Expected: ruby 3.3.4

bundle install
```

To preview the site without changing the shell configuration, run Bundler through rbenv explicitly:

```zsh
RBENV_VERSION=3.3.4 rbenv exec bundle exec jekyll serve --livereload --drafts
```

Preview only published content:

```bash
bundle exec jekyll serve --livereload
```

Preview published content and drafts:

```bash
bundle exec jekyll serve --livereload --drafts
```

Open <http://127.0.0.1:4000/>. Before pushing changes, run:

```bash
bundle exec jekyll build
```

### Troubleshooting

- If `bundle check` reports missing gems, confirm that `ruby -v` shows the version in `.ruby-version`, then run `bundle install` again.
- If Jekyll fails in `pathutil.rb` with `Errno::ENOENT`, check the repository for a broken symbolic link. Remove or move the unrelated link out of the repository, then rebuild with `bundle exec jekyll build`.
- If port `4000` is already in use, choose another port with `bundle exec jekyll serve --port 4001` and open <http://127.0.0.1:4001/>.

## Structure

- `index.html` — homepage
- `blogs/index.html` — automatically generated blog index
- `_drafts/` — unpublished writing; excluded from production builds
- `_posts/` — published posts named `YYYY-MM-DD-slug.md`
- `_templates/post.md` — front matter template for a new post
- `images/blogs/<slug>/` — original images used by a post
- `tools/` — standalone utilities

## Branch workflow

- `master` contains the deployable site infrastructure and published posts.
- `dev` contains work in progress under `_drafts/` and its accompanying images.
- Pull `dev`, edit the Markdown draft directly, preview it locally, then commit and push back to `dev`.
- When a post is ready, move only that post to `_posts/YYYY-MM-DD-<slug>.md`, remove `status: draft`, and merge or cherry-pick its publishing commit into `master`.

The repository is public. A draft on `dev` is not deployed by GitHub Pages, but its source remains visible to anyone who browses that branch on GitHub.

## Blog workflow

1. Switch to `dev`, pull the latest changes, and start or continue `_drafts/<slug>.md` using `_templates/post.md`.
2. Store its images in `images/blogs/<slug>/` and use site paths such as `/images/blogs/<slug>/figure.png`.
3. Preview it with `bundle exec jekyll serve --drafts`.
4. Commit and push draft updates to `dev`.
5. To publish it, remove `status: draft`, move it to `_posts/YYYY-MM-DD-<slug>.md`, and bring that publishing commit into `master`.
6. Run `bundle exec jekyll build`, then push `master`. The Blogs page, feed, and sitemap update automatically.

Files in `_drafts/` are intentionally not part of a normal production build.
