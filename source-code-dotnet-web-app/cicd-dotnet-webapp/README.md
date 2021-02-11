## Run the app locally

Go to the application root folder and run the app using the following commands:
```
cd cicd-dotnet-webapp
dotnet run
```
Then navigate to `http://localhost`


## Push the source code to CodeCommit repo

Add a new git local repository, stage, commit and push:
```
cd cicd-dotnet-webapp
git init
git add .
git commit -m"First commit"
git push -f {outputRepoCloneUrlHttp} master
```
Note, replace ```outputRepoCloneUrlHttp``` with yout actual AWS CodeCommit HTTP URL