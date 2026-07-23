# eov-data-management

Data management guidance for EOVs to be included in the specification sheets.

## Host locally

Need to have Ruby and Jekyll installed on the machine. This site's Gemfile relies on the
`github-pages` gem, which does not yet support Ruby 4.0+, so use Ruby 3.2 (same version as
`.github/workflows/deploy.yml`).

``` bash
bundle exec jekyll serve
```
