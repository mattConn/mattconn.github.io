# My Blog and Webpage
A static site hosted as a Github Pages site. Generated with Hugo, and deployed with Github Actions.
To edit:
- Make changes in the `master` branch
    - To edit the homepage, edit `./themes/etch/layouts/partials/about.md`
    - To edit navlinks, edit `./config.toml`
- Push changes
- A GH Action will then build and deploy this site with Hugo
    - The build will be in the `gh-pages` branch
    - Changes should then be reflected on mattconn.github.io