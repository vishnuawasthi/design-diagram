echo "# design-diagram" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M develop
git remote add origin https://github.com/vishnuawasthi/design-diagram.git
git push -u origin develop

or push an existing repository from the command line
git remote add origin https://github.com/vishnuawasthi/design-diagram.git
git branch -M develop
git push -u origin develop