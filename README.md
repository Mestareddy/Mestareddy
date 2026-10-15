# Save as .github/workflows/metrics.yml in your Mestareddy/Mestareddy profile repo.
# Generates two SVGs that count languages from YOUR commits across every repo
# you've contributed to (including Peerguard-Procurenet and other org repos).

name: Profile metrics

on:
  schedule:
    - cron: "0 3 * * *"   # daily, 04:00 Lagos time
  workflow_dispatch:       # run it manually from the Actions tab
  push:
    branches: [main]
    paths: [".github/workflows/metrics.yml"]

permissions:
  contents: write

jobs:
  languages:
    runs-on: ubuntu-latest
    steps:
      - name: Languages across all contributed repos
        uses: lowlighter/metrics@latest
        with:
          filename: metrics.languages.svg
          token: ${{ secrets.METRICS_TOKEN }}
          committer_token: ${{ secrets.GITHUB_TOKEN }}
          user: Mestareddy
          base: ""
          config_timezone: Africa/Lagos
          plugin_languages: yes
          plugin_languages_indepth: yes            # walks your commits in every repo you've pushed to
          plugin_languages_analysis_timeout: 30
          plugin_languages_sections: most-used, recently-used
          plugin_languages_details: percentage
          plugin_languages_limit: 8
          plugin_languages_threshold: 2%
          plugin_languages_categories: programming, markup
          plugin_languages_recent_categories: programming, markup
          plugin_languages_recent_days: 90
          # Uncomment to hide noise, e.g. generated or vendored code:
          # plugin_languages_ignored: html, css
          # plugin_languages_skipped: some-repo-to-skip

  notable:
    runs-on: ubuntu-latest
    needs: languages
    steps:
      - name: Organizations and repos contributed to
        uses: lowlighter/metrics@latest
        with:
          filename: metrics.notable.svg
          token: ${{ secrets.METRICS_TOKEN }}
          committer_token: ${{ secrets.GITHUB_TOKEN }}
          user: Mestareddy
          base: ""
          config_timezone: Africa/Lagos
          plugin_notable: yes
          plugin_notable_from: all
          plugin_notable_repositories: yes
          plugin_notable_indepth: yes
