---
title: Quick Overview - EF Core
author: rowanmiller
ms.author: divega

ms.date: 10/27/2016

ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core

uid: core/index
---

# Entity Framework Core Quick Overview

Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.

EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write. 

EF Core supports many database engines, see [Database Providers](providers/index.md) for details.

If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.

## What is new in EF Core

If you are familiar with EF Core and want to jump straight into the details of the latest releases:

- **[What is new in EF Core 2.1 (currently in preview)](xref:core/what-is-new/ef-core-2.1)**
- **[What is new in EF Core 2.0 (the latest released version)](xref:core/what-is-new/ef-core-2.0)**
- **[Upgrading existing applications to EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**


## Get Entity Framework Core

[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use. E.g. to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Or in Visual Studio, using the Package Manager Console:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.

## The Model

With EF Core, data access is performed using a model. A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data. See [Creating a Model](modeling/index.md) to learn more.

You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

## Querying

Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ). See [Querying Data](querying/index.md) to learn more.

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## Saving Data

Data is created, deleted, and modified in the database using instances of your entity classes. See [Saving Data](saving/index.md) to learn more.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
