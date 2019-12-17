<h1>Install Sample Application</h1>   
> This document explains how to configure and install the pre-built Synapse platform and Terminus Applications bundled in the repo inside /Interneuron.StarterApp folder. 
>
> The bundle also has a demo data script to load some demo data. 



## Prerequisites

**Deployment** [windows]


1. IIS 7 or higher, please follow instructions on this link to [configure IIS](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/?view=aspnetcore-3.1#iis-configuration) 

2. .NET Core Hosting Bundle [2.0.9](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-aspnetcore-2.0.9-windows-hosting-bundle-installer) and [2.2.6](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-aspnetcore-2.2.6-windows-hosting-bundle-installer) 

3. IIS [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite)

4. [Postgres 11.x](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) 

   


## Install and Configure

1. Install all the Prerequisites. 

   [hint: configure postgres user "postgres" with password as "password" for this sample to work out of the box ]

#### Preparing the install bits

1. Navigate to the folder /Interneuron.StarterApp in the Github repository 

2. Download and copy the "Interneuron" folder into IIS root C:\inetpub\wwwroot

3. This sample application uses the following PostgreSql connection details :

   1. user id =  "postgres"
   2. Password = "password"
   3. port = 5432
   4. server = localhost

    **If you wish to use a different username, password, port or server instance name for your Postgres database,  change the connection strings in these places.**  

   1. open the **studio** folder open web.config and update these connection strings

   ```
       <add name="PGSQLConnection" connectionString="Server=localhost;Port=5432;User Id=postgres;Password=password;Database=synapse;" />
       
           <add name="PGSQLConnectionSIS" connectionString="Server=localhost;Port=5432;User Id=postgres;Password=password;Database=SynapseIdentity;" />
   
   ```

   2. open the **SynapseDynamicAPI** folder open **appsettings.json**  file and update this connection string

   ```
   "ConnectionStrings": {
         "SynapseDataStore": "Server=localhost;Port=5432;User Id=postgres;Password=password;Database=synapse"
       },
   ```

   3. open the **SynapseIdentityServer** folder open **appsettings.json**  file and update this connection strings

   ```
   "ConnectionString": "Server=localhost;Port=5433;User Id=postgres;Password=password;Database=SynapseIdentity"
   ```

   
#### Create Interneuron Sites on IIS

1. Locate and copy the **Interneuron-AppPools.xml** and **Interneuron-Sites.xml** from /Interneuron.StarterApp/IISSettings folder into a local folder.

2. Open a command prompt window in administrator mode and execute the below commands

```
   %windir%\system32\inetsrv\appcmd add apppool /in < "path to Interneuron-AppPools.xml"
   
   %windir%\system32\inetsrv\appcmd add site /in < "path to Interneuron-Sites.xml"
```

3. open IIS manger by typing "inetmgr" in windows run dialog or windows search

4. Expand sites

5. Right click on "Interneuron"--> Click "Edit Bindings" --> Select the https binding and click Edit. Under SSL certificate, expand the dropdown to select **IIS Express Development Certificate** and click ok 

6. Right click on "InterneuronSIS"--> Click "Edit Bindings" --> Select the https binding and click Edit. Under SSL certificate, expand the dropdown to select **IIS Express Development Certificate** and click ok 
   



#### Install the Databases 

* Ensure Postgres 11.x is installed. 
* Locate and copy the SynapseSchema folder from /Interneuron.StarterApp into your local computer.
* Open windows PowerShell or windows command prompt in administrator mode.
* Navigate to Postgres bin folder

 ```powershell
 cd "c:\Program Files\PostgreSQL\11\bin"
 ```

* Install SynapseIdentity Database. 

```sh
psql --host localhost --port 5432 --username postgres --password --file c:\SynapseSchema \RestoreSISSchema_v2.0.sql
```
* Install Synapse Database. 

```sh
psql --host localhost --port 5432 --username postgres --password --file c:\SynapseSchema \RestoreSynapseSchema_v2.0.sql
```
* If you wish to Install Terminus Demo Data.  

```sh
psql --host localhost --port 5432 --username postgres --password --file c:\SynapseSchema \RestoreTerminusDemoData.sql
```

**please change the host, port, user, and file as needed). Enter password whenever prompted.** 



## Usage

* Navigate to [Terminus application](https//localhost:8085/terminus)

* default username is "john.smith" and password is "Pass123$"

* To configure and customise the platform, navigate to [Synapse Studio](https://localhost:8085/studio)

* If you wish to configure ADFS or AAD identity providers, please follow the readme.md in Synapse Identity Server application. 

  

## Author

* GitHub: [Interneuron CIC](https://github.com/InterneuronCIC)



---

## 📝 License

Interneuron Synapse
Copyright(C) 2019  Interneuron CIC

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program. If not, see <http://www.gnu.org/licenses/>.
