# How to create a new .NET Core 5.0 (or later) web application

If you need an already existing web application, then you can get the source code from **cicd-dotnet-webapp** folder. It contains a .NET 5.0 web app for a demo.

Here we are going to explain step by step how to create your own app from scratch within a minute!

### 1. Install .NET

Download and install .NET 5.0 (or later) SDK from [here](https://dotnet.microsoft.com/download)


### 2. Create a new .NET Core web app on your local machine:

Run the following command to create a template:
```
dotnet new webapp -o cicd-dotnet-webapp
```


## 3. Run the app locally

Go to the application root folder and run the app using the following commands:
```
cd cicd-dotnet-webapp
dotnet run
```
Then navigate to `https://localhost:5001`


## 4. Change port to 80

Open **Program.cs** file and add the following code into `CreateHostBuilder()` method:
``` 
webBuilder.UseKestrel(options => { options.Listen(IPAddress.Any, 80); });
```
Your code should look like this:
```
public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseKestrel(options => { options.Listen(IPAddress.Any, 80); });
                    webBuilder.UseStartup<Startup>();
                });
```
Then navigate to `http://localhost`


## 5. Added some extra files:

Copy the following files from **cicd-files-for-webapp** folder and paste them into the root folder of your app:
- buildspec.yml (for CodeBuild)
- appspec.yml (for CodeDeploy)
- codedeploy-scripts than includes 4 sh files (for CodeDeploy)
- .gitignore (for CodeCommit, copied from [here](https://github.com/dotnet/core/blob/master/.gitignore))


## 6. Push the source code to CodeCommit repo

Add a new git local repository, stage, commit and push:
```
cd cicd-dotnet-webapp
git init
git add .
git commit -m"First commit"
git push -f {outputRepoCloneUrlHttp} master
```
Note, replace ```outputRepoCloneUrlHttp``` with yout actual AWS CodeCommit HTTP URL


# Useful articles and notes:
:point_right: **Fix - How can I run a dotnet application in the background using sudo on Linux?**

So if you run 
```
cd ../home/ec2-user/cicd-dotnet-webapp && sudo dotnet cicd-dotnet-webapp.dll
``` 
in EC2 instance, then the code will run till you are SHHed to the instance. Once you close your EC2 intance connection, the app will be stoped. Use ```screen``` command instead:
```
screen -d -m -S SERVER bash -c 'cd ../home/ec2-user/cicd-dotnet-webapp && sudo dotnet cicd-dotnet-webapp.dll'
```

Find details [here](https://stackoverflow.com/questions/49479635/how-can-i-run-a-dotnet-application-in-the-background-using-sudo-on-linux).

:point_right: **Issue - CodeBuild does NOT support .NET 5.0**

Error `AWS Codebuild: Unknown runtime version named '5.0' of dotnet`.

The latest supported version by AWS CodeBuild is dotnet 3.1

Read more:

:blue_book: AWS Codebuild: Unknown runtime version named '5.0' of dotnet [source](https://stackoverflow.com/questions/65546757/aws-codebuild-unknown-runtime-version-named-5-0-of-dotnet) 

:blue_book: .NET 5 Release - runtime support [source](https://github.com/aws/aws-codebuild-docker-images/issues/401)

:point_right: **Dotnet documentation**

:blue_book: .NET docs [source](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new)