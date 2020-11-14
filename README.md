# MariaDbASPSetup
implementation of Maria Db for ASP.net webApp.


Use this as a reference in setting up Maria DB on remote server(QNAP) with ASP.NET CORE

Implementation Details...

QNAP Configuration: 

Setup MariaDb on remote server
Install 


ASP.NET Configuration:

Visual Studio 2019 --> ASP.NET Core Web Application --> [enter project name] -->  Web Application [configure for HTTPS [checked]]

(A project template for creating an ASP.NET Core application with example ASP.NET Razor Pages content.)

Dependencies -  (reference package dependency version requirements)
  - Microsoft.EntityFrameworkCore.Relational (3.1.8)
  - Microsoft.EntityFrameworkCore.Tools (3.1.8)
  - MySqlConnector (0.69.10)
  - Pomelo.EntityFrameworkCore.MySql(3.2.4)
  - Pomelo.JsonObject (2.2.1)
  
  ----------------------------------------------------------------
 appsettings.Developement.json:
 
  "ConnectionStrings": {
    "PlutoConnectionString": "server=192.168.0.***,port;user id=cannella;password=********;database=aspnetcore.mariadb"
  },
  ---------------------------------------------------------------
Add DbContext Class:


    public class PlutoContext : DbContext
    { 
        public DbSet<Course> Courses { get; set; }
        public DbSet<Author> Authors { get; set; }
        public DbSet<Tag> Tags { get; set; }
        public PlutoContext(DbContextOptions<PlutoContext> options)
            :base (options)
        {

        }
    }
    
      -------------------------------------------------------
 Add Services to Startup.cs
 
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContext<PlutoContext>(options =>
                options.UseMySql(Configuration.GetConnectionString("PlutoConnectionString"),
                mySqlOptionsAction => mySqlOptionsAction.ServerVersion(new Version(10, 5, 4), ServerType.MariaDb)
                )

            );
            services.AddRazorPages();
        }
