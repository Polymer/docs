## Polymer documentation site

https://www.polymer-project.org/

### Install

    git clone https://github.com/Polymer/docs --recursive
    cd docs
    npm install

Note: `--recursive` will init and checkout submodules as well.

If you have cloned and built an earlier version of the doc site (before June 2016), note
that the site has changed and **the master branch shares no history with the old master 
branch.** The easiest way to get up and running with the new site is to create a brand new
repo as described above, to avoid introducing any artifacts from the old doc site. 

You can find the old doc site (including Polymer 0.5 docs) in the
[docs-05](https://github.com/Polymer/docs/tree/docs-05) branch.

### Running the site

The first time you run the site, run `gulp` to build the site in its entirety:

    gulp

Then start the App Engine dev server on `dist/app.yaml`:

    dev_appserver.py .

The site will be served from http://localhost:8080.

#### Making changes / watching files / live reload

If you're making changes use the `watch` task. Optionally add `--reload` to live
reload the tab when changes are saved.

    gulp watch --reload

The site will be served on http://localhost:3000. Making changes will refresh
the browser tab.

Optional flags:

- `--reload`: refreshes the browser tab when changes are made
- `--open`: opens a new browser tab when `gulp watch` is started

**Tip** - run `gulp help` to see the list of available gulp tasks.

### Run tests

Install [WebTest framework](http://webtest.pythonpaste.org/en/latest/):

    pip install WebTest

Then run:

    npm test

### Deployment

Build and deploy version `YYY-MM-DD` of the site: 

    gulp
    npm run deploy YYYY-MM-DD
