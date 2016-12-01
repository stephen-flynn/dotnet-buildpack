**Please note:** Forked from [noliar](https://github.com/noliar/dotnet-buildpack) which forked it from [Heroku](https://github.com/heroku/dotnet-buildpack)

# ASP.NET Core Buildpack for Heroku

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpack) for building [ASP.NET Core](https://docs.asp.net/en/latest/conceptual-overview/aspnet.html) apps using [`project.json` files](https://github.com/aspnet/Home/wiki/Project.json-file) and the [.NET CLI](https://github.com/dotnet/cli).

## Usage

Example usage:

    $ heroku create --buildpack http://github.com/jenyayel/dotnet-buildpack.git
    $ git push heroku master

The buildpack will detect your app as ASP.NET Core if it has `project.json`. If the source code you want to build contains multiple `project.json` files, you can use a [`.deployment`](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) or set a `$PROJECT` config var to control which one is built.

## Side notes
1. Your web project should use `Microsoft.Extensions.Configuration.CommandLine` package in order to pass `--server.urls` to executable.
2. You must have `.deployment` and `Procfiles` files in root of the solution

### .deployment file
This one instructs which projects need to be deployed. For single executable use:
```
[config]
project = src/WebAPI
```

For multiple executables:
```
[config]
project = src/WebAPI
project = src/Worker
```

### Procfile file
This file instructs Heroku how dynos mapped to executable files. Here is an example:
```
web: cd $HOME/heroku_output/WebAPI && dotnet ./WebAPI.dll --server.urls http://+:$PORT ${CORE_ENVIRONMENT}
worker: cd $HOME/heroku_output/Worker && dotnet ./Worker.dll
```