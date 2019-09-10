![Actions Status](https://github.com/qdimka/EntityFramework.DataProtection/workflows/build/badge.svg)
![Actions Status](https://github.com/qdimka/EntityFramework.DataProtection/workflows/release/badge.svg)
![Nuget](https://img.shields.io/nuget/v/EF.DataProtection.Core)
![Nuget](https://img.shields.io/nuget/dt/EF.DataProtection.Core)

# EF.DataEncryption
+ Flexible
+ Declarative
+ Linq support (with limitations)
+ Supports netstandard 2.0

# Status
This library is still under construction and needs to be peer reviewed as well as have features added.

# Usage
Add reference to EntityFramework.DataProtection.Core to your project.

```
dotnet add package EF.DataProtection.Core
```

Include the following code to Startup.cs

```csharp
  services
      .AddEntityFrameworkSqlite()
      .AddDbContext<SampleDbContext>(
          (p, opt) =>
          {
              opt.UseSqlite(_connection)
                  .UseInternalServiceProvider(p);
          })
      .AddEfEncryption()
      .UseAes256(opt =>
      {
          opt.Password = "Really_Strong_Password_For_Data";
          opt.Salt = "Salt";
      })
      .UseSha512(opt => { opt.Password = "Really_Strong_Password_For_Data"; });
```

Mark properties in your entity with attributes `Aes256` or `Sha512`

```csharp
  public class PersonalData
  {
      [Aes256]
      public string PhoneNumber { get; set; }

      [Aes256]
      public string Email { get; set; }

      [Aes256]
      public string SensitiveData { get; set; }

      [Sha512]
      public string PhoneNumberHash { get; protected set; }
  }
```

Enjoy!

# Limitations
+ Only string supported
+ Sha512 doesn`t return value after hashing
+ There is no way to search by encrypted value. Use hash for search.

# Maintainers
+ [qdimka](https://github.com/qdimka)
