## Introduction and Resources

### Resource Groups (East US) :
* Primary Resource Group - dotNet-azSQL-<YOUR-INITIALS-HERE>
	
### Server Names (East US) : 
* SQL ServerName - sql-dotnet-azsql-<YOUR-INITIALS-HERE>
    * Credentials : --admin-user wwt-admin --admin-password <STRONG-PASSWORD>
    * Connection String : "Server=tcp:sql-dotnet-azsql-<YOUR-INITIALS-HERE>.database.windows.net,1433;Database=coreDB;User ID=wwt-admin;Password=<STRONG-PASSWORD>;Encrypt=true;Connection Timeout=30;"

## Setting Up Azure, Creating your SQL DB and Connecting to your .NET Application

1. Let's begin by creating a Resource Group for your application
```az group create --name dotNet-azSQL-<YOUR-INITIALS-HERE> --location "East US"```

**Output**
```
{
  "id": "/subscriptions/a9684ba4-61f2-4e72-b44e-126d096dd8a3/resourceGroups/dotNet-azSQL-<YOUR-INITIALS-HERE>",
  "location": "eastus",
  "managedBy": null,
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
```

2. Next we'll provision your SQL server in Azure, be sure to use a password of high complexity.
*It's recommended you use the below naming convention so your resources are easily found*
```az sql server create --name sql-dotnet-azsql-<YOUR-INITIALS-HERE> --resource-group dotNet-azSQL-<YOUR-INITIALS-HERE> --location "East US" --admin-user wwt-admin --admin-password <STRONG-PASSWORD>```

**Output**
```
{
  "administratorLogin": "wwt-admin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "sql-dotnet-azsql-<YOUR-INITIALS-HERE>.database.windows.net",
  "id": "/subscriptions/a9684ba4-61f2-4e72-b44e-126d096dd8a3/resourceGroups/dotNet-azSQL-<YOUR-INITIALS-HERE>/providers/Microsoft.Sql/servers/sql-dotnet-azsql-<YOUR-INITIALS-HERE>",
  "identity": null,
  "kind": "v12.0",
  "location": "eastus",
  "minimalTlsVersion": null,
  "name": "sql-dotnet-azsql-<YOUR-INITIALS-HERE>",
  "privateEndpointConnections": [],
  "publicNetworkAccess": "Enabled",
  "resourceGroup": "dotNet-azSQL-<YOUR-INITIALS-HERE>",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

3. Next we'll set the firewall rules to keep your SQL server secure and allow access only internally to Azure.
```az sql server firewall-rule create --resource-group dotNet-azSQL-<YOUR-INITIALS-HERE> --server sql-dotnet-azsql-<YOUR-INITIALS-HERE> --name AllowAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0```

**Output**
```
{
  "endIpAddress": "0.0.0.0",
  "id": "/subscriptions/a9684ba4-61f2-4e72-b44e-126d096dd8a3/resourceGroups/dotNet-azSQL-<YOUR-INITIALS-HERE>/providers/Microsoft.Sql/servers/sql-dotnet-azsql-<YOUR-INITIALS-HERE>/firewallRules/AllowAzureIps",
  "kind": "v12.0",
  "location": "East US",
  "name": "AllowAzureIps",
  "resourceGroup": "dotNet-azSQL-<YOUR-INITIALS-HERE>",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.Sql/servers/firewallRules"
}
```

4. Then we'll provision access for your local IP address to your SQL server on Azure.
```az sql server firewall-rule create --name AllowLocalClient --server sql-dotnet-azsql-<YOUR-INITIALS-HERE> --resource-group dotNet-azSQL-<YOUR-INITIALS-HERE> --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address>```

**Output**
```
{
  "endIpAddress": "your-ip-address",
  "id": "/subscriptions/a9684ba4-61f2-4e72-b44e-126d096dd8a3/resourceGroups/dotNet-azSQL-<YOUR-INITIALS-HERE>/providers/Microsoft.Sql/servers/sql-dotnet-azsql-<YOUR-INITIALS-HERE>/firewallRules/AllowLocalClient",
  "kind": "v12.0",
  "location": "East US",
  "name": "AllowLocalClient",
  "resourceGroup": "dotNet-azSQL-<YOUR-INITIALS-HERE>",
  "startIpAddress": "your-ip-address",
  "type": "Microsoft.Sql/servers/firewallRules"
}
```

5. Now we'll create the database for the SQL server to store data.
```az sql db create --resource-group dotNet-azSQL-<YOUR-INITIALS-HERE> --server sql-dotnet-azsql-<YOUR-INITIALS-HERE> --name coreDB --service-objective S0```

**Output**
```
{
  "autoPauseDelay": null,
  "catalogCollation": "SQL_Latin1_General_CP1_CI_AS",
  "collation": "SQL_Latin1_General_CP1_CI_AS",
  "createMode": null,
  "creationDate": "2020-09-22T18:54:02.103000+00:00",
  "currentServiceObjectiveName": "S0",
  "currentSku": {
    "capacity": 10,
    "family": null,
    "name": "Standard",
    "size": null,
    "tier": "Standard"
  },
  "databaseId": "cd1deaed-22e6-4cea-9908-b406bc789dcf",
  "defaultSecondaryLocation": "westus",
  "earliestRestoreDate": "2020-09-22T19:24:02.103000+00:00",
  "edition": "Standard",
  "elasticPoolId": null,
  "elasticPoolName": null,
  "failoverGroupId": null,
  "id": "/subscriptions/a9684ba4-61f2-4e72-b44e-126d096dd8a3/resourceGroups/dotNet-azSQL-<YOUR-INITIALS-HERE>/providers/Microsoft.Sql/servers/sql-dotnet-azsql-<YOUR-INITIALS-HERE>/databases/coreDB",
  "kind": "v12.0,user",
  "licenseType": null,
  "location": "eastus",
  "longTermRetentionBackupResourceId": null,
  "managedBy": null,
  "maxLogSizeBytes": null,
  "maxSizeBytes": 268435456000,
  "minCapacity": null,
  "name": "coreDB",
  "pausedDate": null,
  "readReplicaCount": 0,
  "readScale": "Disabled",
  "recoverableDatabaseId": null,
  "recoveryServicesRecoveryPointId": null,
  "requestedServiceObjectiveName": "S0",
  "resourceGroup": "dotNet-azSQL-<YOUR-INITIALS-HERE>",
  "restorableDroppedDatabaseId": null,
  "restorePointInTime": null,
  "resumedDate": null,
  "sampleName": null,
  "sku": {
    "capacity": 10,
    "family": null,
    "name": "Standard",
    "size": null,
    "tier": "Standard"
  },
  "sourceDatabaseDeletionDate": null,
  "sourceDatabaseId": null,
  "status": "Online",
  "storageAccountType": "GRS",
  "tags": null,
  "type": "Microsoft.Sql/servers/databases",
  "zoneRedundant": false
}
```

6. We need to query Azure for your SQL Database connection string below, be sure to replace the credentials below with your credentials.
```az sql db show-connection-string --client ado.net --server sql-dotnet-azsql-<YOUR-INITIALS-HERE> --name coreDB```

**Output**
```"Server=tcp:sql-dotnet-azsql-<YOUR-INITIALS-HERE>.database.windows.net,1433;Database=coreDB;User ID=wwt-admin;Password=<STRONG-PASSWORD>;Encrypt=true;Connection Timeout=30;"```

7. Go into your applicaton folder and open the Startup.cs file. Search for the code :
```
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

8. Replace this code segment with the new lines below
```
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
```

**After replacing the code we'll want to update our Migrations to support linking the local application to your Azure SQL Database.**

9. 
```dotnet ef Migrations remove```

```
Build started...
Build succeeded.
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 3.1.3 initialized 'MyDatabaseContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (33ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT 1
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (30ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT OBJECT_ID(N'[__EFMigrationsHistory]');
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (21ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT [MigrationId], [ProductVersion]
      FROM [__EFMigrationsHistory]
      ORDER BY [MigrationId];
Removing migration '20200923150039_InitialCreate'.
Removing model snapshot.
Done.
```

-

```dotnet ef migrations add InitialCreate```

```
Build started...                                                                                                                     
Build succeeded.
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 3.1.3 initialized 'MyDatabaseContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

-

```
# PowerShell
$env:ConnectionStrings:MyDbConnection="Server=tcp:sql-dotnet-azsql-<YOUR-INITIALS-HERE>.database.windows.net,1433;Database=coreDB;User ID=wwt-admin;Password=<STRONG-PASSWORD>;Encrypt=true;Connection Timeout=30;"
# CMD (no quotes)
set ConnectionStrings:MyDbConnection=Server=tcp:sql-dotnet-azsql-<YOUR-INITIALS-HERE>.database.windows.net,1433;Database=coreDB;User ID=wwt-admin;Password=<STRONG-PASSWORD>;Encrypt=true;Connection Timeout=30;
# Bash (no quotes)
export ConnectionStrings__MyDbConnection=Server=tcp:sql-dotnet-azsql-<YOUR-INITIALS-HERE>.database.windows.net,1433;Database=coreDB;User ID=wwt-admin;Password=<STRONG-PASSWORD>;Encrypt=true;Connection Timeout=30;
```

-

```dotnet ef database update```

```
Build started...
Build succeeded.
info: Microsoft.EntityFrameworkCore.Infrastructure[10403]
      Entity Framework Core 3.1.3 initialized 'MyDatabaseContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (33ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT 1
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (34ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT OBJECT_ID(N'[__EFMigrationsHistory]');
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (32ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT 1
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT OBJECT_ID(N'[__EFMigrationsHistory]');
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (22ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT [MigrationId], [ProductVersion]
      FROM [__EFMigrationsHistory]
      ORDER BY [MigrationId];
info: Microsoft.EntityFrameworkCore.Migrations[20402]
      Applying migration '20200923150507_InitialCreate'.
Applying migration '20200923150507_InitialCreate'.
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (22ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [Todo] (
          [ID] int NOT NULL IDENTITY,
          [Description] nvarchar(max) NULL,
          [CreatedDate] datetime2 NOT NULL,
          CONSTRAINT [PK_Todo] PRIMARY KEY ([ID])
      );
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (22ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20200923150507_InitialCreate', N'3.1.3');
Done.
```

## Configuring your WebApp Deployment
-

```az webapp deployment user set --user-name <your-user-name> --password <YOUR-INITIALS-HERE>WWTwwt1!```

```
{
  "id": null,
  "kind": null,
  "name": "web",
  "publishingPassword": null,
  "publishingPasswordHash": null,
  "publishingPasswordHashSalt": null,
  "publishingUserName": "stray",
  "scmUri": null,
  "type": "Microsoft.Web/publishingUsers/web"
}
```

-

```az appservice plan create --name app-dotnet-azsql-<YOUR-INITIALS-HERE>  --resource-group dotNet-azSQL-<YOUR-INITIALS-HERE> --sku F1 --is-linux```

```
{
  "freeOfferExpirationTime": null,
  "geoRegion": "East US",
  "hostingEnvironmentProfile": null,
  "hyperV": false,
  "id": "/subscriptions/a9684ba4-61f2-4e72-b44e-126d096dd8a3/resourceGroups/dotNet-azSQL-<YOUR-INITIALS-HERE>/providers/Microsoft.Web/serverfarms/app-dotnet-azsql-<YOUR-INITIALS-HERE>",
  "isSpot": false,
  "isXenon": false,
  "kind": "linux",
  "location": "East US",
  "maximumElasticWorkerCount": 1,
  "maximumNumberOfWorkers": 1,
  "name": "app-dotnet-azsql-<YOUR-INITIALS-HERE>",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "dotNet-azSQL-<YOUR-INITIALS-HERE>",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "F",
    "locations": null,
    "name": "F1",
    "size": "F1",
    "skuCapacity": null,
    "tier": "Free"
  },
  "spotExpirationTime": null,
  "status": "Ready",
  "subscription": "a9684ba4-61f2-4e72-b44e-126d096dd8a3",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```