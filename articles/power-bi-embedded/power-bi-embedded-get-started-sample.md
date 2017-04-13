<properties
   pageTitle="Valimi kasutamise alustamine"
   description="Power BI manustatud, SDK abil saate lisada Power BI interaktiivsete aruannete business intelligence rakendusse"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-power-bi-embedded-sample"></a>Power BI manustatud valimi kasutamise alustamine

**Microsoft Power BI manustatud**, kus saate integreerida otse oma veebis või mobiilirakenduste Power BI aruanded. Selles artiklis me tutvustada teile **Power BI manustatud** toomine alustamine valimi.

Enne kui me minna edasi, soovite võib-olla salvestamiseks järgmistest ressurssidest. Aitab see teil kui integreerimine valimi rakendus ja oma rakenduste Power BI aruannete liiga.

 -  [Valimi armatuurlaua web app](http://go.microsoft.com/fwlink/?LinkId=761493)
 -  [Power BI manustatud API viide](https://msdn.microsoft.com/library/mt711493.aspx)
 -  [Power BI manustatud .NET SDK](http://go.microsoft.com/fwlink/?LinkId=746472) (saadaval Nugeti kaudu)



> [AZURE.NOTE] Enne saate konfigureerida ja käivitada Power BI manustatud toomine alustamine valimi, peate looma vähemalt ühte **Tööruumi saidikogumi** teie Azure'i tellimus. Siit saate teada, kuidas luua leiate Azure'i portaalis **Tööruumi saidikogumi** [Alustamine Power BI manustatud](power-bi-embedded-get-started.md).

## <a name="configure-the-sample-app"></a>Valimi rakenduse konfigureerimiseks

Vaatame häälestada oma Visual Studio arenduskeskkond juurdepääsu valimi rakenduse käivitamiseks komponendid.

1. Laadige alla ja pakkige see lahti [Power BI manustatud - integreerida aruande web appi](http://go.microsoft.com/fwlink/?LinkId=761493) valimi github.

2. Avage **PowerBI-embedded.sln** Visual Studios. Võimalik, et peate käsu **Update pakett** Nugeti Package Manager konsooli värskendamiseks kasutada seda lahendust paketid.

3. Koostada lahendus.

4. Käivitage rakendus **ProvisionSample** konsooli. Rakenduse valimi konsooli tööruumi ettevalmistamine ja PBIX faili importimine.

5. Uue **tööruumi**ettevalmistamise, valige suvand 5, **sätte mõne olemasoleva tööruumi saidikogumi uue tööruumi**.

    ![](media\powerbi-embedded-get-started-sample\console-option-5.png)

6. Sisestage oma **Tööruumi** nimi ja **Juurdepääsu võti**. Saate need **Azure portaali**. Lisateavet selle kohta, kuidas **Kiirklahv**leiate teemast [Vaate Power BI API kiirklahvide](power-bi-embedded-get-started-sample.md#view-access-keys) rakenduses Microsoft Power BI manustatud alustamine.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

7. Kopeerige ja salvestage vastloodud **Tööruumi ID** selle artikli kasutamiseks. **Tööruumi ID** on loodud, leiate selle **Azure portaali**.

    ![](media\powerbi-embedded-get-started-sample\workspace-id.png)

8. **Tööruumi**PBIX faili importimine, valige suvand **6. Impordifaili PBIX töölaua võtta mõne olemasoleva tööruumi**. Kui teil pole faili PBIX mugav, saate alla laadida selle [jaemüügi analüüsi valimi PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547).

9. Kui kuvatakse vastav viip, sisestage oma **andmekomplekti**sõbralik nimi.

Peaksite nägema umbes vastuse:

````
Checking import state... Publishing
Checking import state... Succeeded
```

> [AZURE.NOTE] If your PBIX file contains any direct query connections, run option 7 to update the connection strings.

At this point, you have a Power BI PBIX report imported into your **Workspace**. Now, let's look at how to run the **Power BI Embedded** get started sample web app.

## Run the sample web app

The web app sample is a sample dashboard that renders reports imported into your **Workspace**. Here's how to configure the web app sample.

1. In the **PowerBI-embedded** Visual Studio solution, right click the **EmbedSample** web application, and choose **Set as StartUp project**.
2. In **web.config**, in the **EmbedSample** web application, edit the **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Run the **EmbedSample** web application.

Once you run the **EmbedSample** web application, the left navigation panel should contain a **Reports** menu. To view the report you imported, expand **Reports**, and click a report. If you imported the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), the sample web app would look like this:

![](media\powerbi-embedded-get-started-sample\power-bi-embedded-sample-left-nav.png)

After you click a report, the **EmbedSample** web application should look something this:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)


## Explore the sample code
The **Microsoft Power BI Embedded** sample is an example dashboard web app that shows you how to integrate **Power BI** reports into your app. It uses a Model-View-Controller (MVC) design pattern to demonstrate best practices. This section highlights parts of the sample code that you can explore within the **PowerBI-embedded** web app solution. The Model-View-Controller (MVC) pattern separates the modeling of the domain, the presentation, and the actions based on user input into three separate classes: Model, View, and Control. To learn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).

The **Microsoft Power BI Embedded** sample code is separated as follows. Each section includes the file name in the PowerBI-embedded.sln solution so that you can easily find the code in the sample.

> [AZURE.NOTE] This section is a summary of the sample code that shows how the code was written. To view the complete sample, please load the PowerBI-embedded.sln solution in Visual Studio.

### Model
The sample has a **ReportsViewModel** and **ReportViewModel**.

**ReportsViewModel.cs**: Represents Power BI Reports.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: Represents a Power BI Report.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### Connection string
The connection string must be in the following format:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Using common server and database attributes will fail. For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### View
The **View** manages the display of Power BI **Reports** and a Power BI **Report**.

**Reports.cshtml**: Iterate over **Model.Reports** to create an **ActionLink**. The **ActionLink** is composed as follows:

|Part|Description
|---|---
|Title| Name of the Report.
|QueryString| A link to the Report ID.

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

Report.cshtml: Set the **Model.AccessToken**, and the Lambda expression for **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### Controller

**DashboardController.cs**: Creates a PowerBIClient passing an **app token**. A JSON Web Token (JWT) is generated from the **Signing Key** to get the **Credentials**. The **Credentials** are used to create an instance of **PowerBIClient**. Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

    public ActionResult Reports()
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


Task<ActionResult> Report(string reportId)

    public async Task<ActionResult> Report(string reportId)
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### Integrate a report into your app

Once you have a **Report**, you use an **IFrame** to embed the Power BI **Report**. Here is a code snippet from  powerbi.js in the **Microsoft Power BI Embedded** sample.

![](media\powerbi-embedded-get-started-sample\power-bi-embedded-iframe-code.png)


## Filter reports embedded in your application

You can filter an embedded report using a URL syntax. To do this, you add a **$filter** query string parameter with an **eq** operator to your iFrame src url with the filter specified. Here is the filter query syntax:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [AZURE.NOTE] {tableName/fieldName} cannot include spaces or special characters. The {fieldValue} accepts a single categorical value.  


## See also

- [Common Microsoft Power BI Embedded scenarios](power-bi-embedded-scenarios.md)
- [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md)
