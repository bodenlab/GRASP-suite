# GRASP-suite

This is a repository of the code used to generate [GRASP-suite](https://bodenlab.github.io/GRASP-suite/).

It is a Hugo site.

Assuming you have Hugo installed on your computer, you should be able to update the website by -

- Make changes in the `content` folder
- Run the `deploy.sh` script in the top folder
- This script will delete the current docs folder, rebuild hugo, then change into the docs folder and commit and push the changes to the repo
- Note: This doesn't push changes automatically in the rest of the repo, so you should still manually add changes that you make outside of the docs folder and push to git normally