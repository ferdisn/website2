`_config.yml` is the main build config for GitHub Pages of this website.
`_config.local.yml` is additional config to override certain variables for the purpose of local testing.

To run local web server, run this command:
`$ bundle exec jekyll serve --config _config.yml,_config.local.yml`

To build the site for others to review (upload to DigitalOcean), run this command:
`$ bundle exec jekyll build --config _config.yml,_config.test.yml`
