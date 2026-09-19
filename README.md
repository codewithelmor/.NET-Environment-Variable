To configure the `ASPNETCORE_ENVIRONMENT` variable for a .NET Web API deployed in **IIS**, you have three primary methods: 

### Method 1: Update the `web.config` File (Most Common)
You can directly tell the **ASP.NET Core Module** inside IIS which environment to use by editing the site's `web.config` file. 

1. Open the `web.config` file in your application's root directory.
2. Inside the `<aspNetCore>` tag, add an `<environmentVariables>` block:

```xml
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" arguments=".\YourApp.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\stdout" hostingModel="inprocess">
      <environmentVariables>
        <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Staging" />
      </environmentVariables>
    </aspNetCore>
  </system.webServer>
</configuration>
```
*Change `value="Staging"` to `Production`, `Development`, or your custom environment name.*

---

### Method 2: Configure via IIS Manager UI (No Code Changes)
If you do not want to alter your deployed files, you can inject it straight through the **IIS Manager Configuration Editor**.

1. Open **IIS Manager** and click on your specific website or application in the left panel.
2. In the main features view, double-click **Configuration Editor**.
3. In the **Section** drop-down at the top, navigate to: `system.webServer/aspNetCore`.
4. Find the **environmentVariables** row and click the **`...`** (ellipse) button on the far right.
5. Click **Add** in the Actions pane and enter:
   * **Name:** `ASPNETCORE_ENVIRONMENT`
   * **Value:** `Staging` (or your chosen environment)
6. Close that pop-up window and click **Apply** in the top right corner of IIS Manager.

---

### Method 3: Automate via Visual Studio Publish Profile
If you want this configured automatically every time you publish from Visual Studio, update your deployment profile.

1. Expand **Properties** > **PublishProfiles** in your project tree and open your `.pubxml` file.
2. Add the `<EnvironmentName>` property inside the `<PropertyGroup>` tag:

```xml
<PropertyGroup>
  <EnvironmentName>Staging</EnvironmentName>
</PropertyGroup>
```
When you run the publish command, Visual Studio will automatically generate the correct `<environmentVariable>` block inside the destination `web.config` file.

---

### Bonus: How to do it in Azure App Service
For comparison, if you ever need to set or update this variable in **Azure App Service**, you do not modify code files. Instead, you manage it via environment variables in the cloud portal:

1. Log into the **Azure Portal** and navigate to your **App Service**.
2. In the left-hand navigation menu, under **Settings**, click on **Environment variables** (formerly *Configuration*).
3. Under the **App settings** tab, click **+ Add** (or click the existing variable to edit it).
4. Enter the following details:
   * **Name:** `ASPNETCORE_ENVIRONMENT`
   * **Value:** `Staging` (or `Production`, `Development`)
5. Click **Apply** at the bottom of the blade, then click **Apply** again at the top of the main screen to save and automatically restart your application with the new configuration.
