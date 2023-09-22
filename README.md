## Local Setup 

$ bundle install
$ pip install jupyter
$ bundle exec jekyll serve --lsi


## deploy

#### It will automatically re-deploy webpage each time you push new changes.

To enable automatic deployment:
1. Click on Actions tab and Enable GitHub Actions;  everything has already been set for you.
2. Go to Settings -> Actions -> General -> Workflow permissions, and give Read and write permissions to GitHub Actions
3. Make any other changes to your webpage, commit, and push. This will automatically trigger the Deploy action.
4. Wait for a few minutes and let the action complete. You can see the progress in the Actions tab. If completed successfully, in addition to the main branch, your repository should now have a newly built gh-pages branch.
5. In the Settings of your repository, in the Pages section, set the branch to gh-pages (NOT to main).

#### Manual deploy

After you pushing the change to repo, and then run **bin/deploy** script.


## using 

- Github Page
- Jekyll 
- Bootstrap4

