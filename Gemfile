source "https://rubygems.org"

# --- Local development only -------------------------------------------------
# Production deploys via *classic* GitHub Pages, which builds the site
# server-side with its own pinned `github-pages` gem and IGNORES this Gemfile.
# So we use modern Jekyll 4 here (it builds cleanly on current Ruby/macOS,
# unlike the ancient github-pages gemset). The site's SCSS is self-contained
# and the layouts are overridden locally, so local/prod rendering stays in sync.
#
# Run with Homebrew's Ruby (system Ruby 2.6 is too old):
#   export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
#   bundle install
#   bundle exec jekyll serve
# ---------------------------------------------------------------------------

gem "jekyll", "~> 4.4"
gem "minima", "~> 2.5"

# Keep this list matching `plugins:` in _config.yml.
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.17"
  gem "jekyll-paginate", "~> 1.1"
  gem "jekyll-redirect-from", "~> 0.16"
  gem "jekyll-sitemap", "~> 1.4"
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]
# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.0" if Gem.win_platform?

# Gems that left Ruby's default set in 3.4+/4.0 but Jekyll still expects.
gem "webrick", "~> 1.9"
gem "csv"
gem "base64"
gem "logger"
gem "bigdecimal"
