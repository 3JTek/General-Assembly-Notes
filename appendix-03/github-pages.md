# Github Pages

Github allows you to host a static site for each repository you create. This is useful for hosting documentation, exmaple usage of software, or a standard static website.

## Setup

Create a new branch called gh-pages:

```
gco -b gh-pages
```

Push the branch to github:

```
git push origin gh-pages
```

View your hosted project at the following url:

`https://username.github.io/reponame`

So for example if your github username is mickyginger and your repo name is WDI_LDN_PROJECT1, your site would be available at:

`https://mickyginger.github.io/WDI_LDN_PROJECT1`

To update the project, make sure you are working on the master branch. Switch back to master:

```
gco master
```

Once you have updated the project make sure you commit and push your updates:

```
git add .
git commit -m "Updated project"
git push origin master
```

Now you can merge those changes into the gh-pages branch and push them to update the project online:

```
gco gh-pages
git merge master
git push origin gh-pages
```

## Further reading

* [Github Pages](https://pages.github.com/)
