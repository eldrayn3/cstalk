dotnet new sln -n WebApiSolution

dotnet new classlib -n Domain
dotnet new classlib -n Data
dotnet new classlib -n Services

dotnet new webapi -n WebApiApp

dotnet sln add Domain
dotnet sln add Data
dotnet sln add Services
dotnet sln add WebApiApp

dotnet add Data reference Domain
dotnet add Services reference Data
dotnet add Services reference Domain

dotnet add WebApiApp reference Domain
dotnet add WebApiApp reference Data
dotnet add WebApiApp reference Services

dotnet add Data package Microsoft.EntityFrameworkCore
dotnet add Data package Microsoft.EntityFrameworkCore.InMemory

dotnet add WebApiApp package Microsoft.EntityFrameworkCore
dotnet add WebApiApp package Microsoft.EntityFrameworkCore.InMemory

