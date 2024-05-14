1. Setting Up the ASP.NET Core Web Application
what you need
Install .NET SDK.
Install Visual Studio or Visual Studio Code.
Install MySQL.
Install Git.
Create ASP.NET Core Project
> dotnet new webapp -n FoodApp
> cd FoodApp

Add Required Packages
> dotnet add package Microsoft.EntityFrameworkCore
> dotnet add package Pomelo.EntityFrameworkCore.MySql

Set Up the Model and DbContext
> Model - Add a FoodItem model.
> Create a folder named Models and add a file FoodItem.cs

DbContext
> Create a folder named Data and add a file ApplicationDbContext.cs

2. Dockerize the Application
3. Deployment Scripts
Update Startup Configuration:
> Modify Startup.cs to use your DbContext and add a connection string in appsettings.json

Scaffold Pages Using Entity Framework:
Use the following command to scaffold pages for CRUD operations
> dotnet tool install --global dotnet-aspnet-codegenerator
> dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
> dotnet aspnet-codegenerator razorpage -m FoodItem -dc ApplicationDbContext -udl -outDir Pages/FoodItems --referenceScriptLibraries

2. Dockerize the Application
Create a Dockerfile in the root of your project
> FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
> WORKDIR /app
> EXPOSE 80

> FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
> WORKDIR /src
> COPY ["FoodApp.csproj", "./"]
> RUN dotnet restore "FoodApp.csproj"
> COPY . .
> RUN dotnet build "FoodApp.csproj" -c Release -o /app/build

> FROM build AS publish
> RUN dotnet publish "FoodApp.csproj" -c Release -o /app/publish

> FROM base AS final
> WORKDIR /app
> COPY --from=publish /app/publish .
> ENTRYPOINT ["dotnet", "FoodApp.dll"]

3. Create Deployment Scripts and remove scripts
