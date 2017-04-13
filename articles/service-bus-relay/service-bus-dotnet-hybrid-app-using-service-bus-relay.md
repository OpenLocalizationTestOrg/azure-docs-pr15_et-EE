<properties
    pageTitle="Hübriidjuurutuse kohta – ruumide/pilve rakendus (.NET) | Microsoft Azure'i"
    description="Saate teada, kuidas luua .net-i kohta – ruumide/cloud hübriid rakenduse abil Azure'i teenus siini relay."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.Net-i kohta – ruumide/cloud hübriid rakenduse abil Azure'i teenus siini edastamine

## <a name="introduction"></a>Sissejuhatus

Selles artiklis kirjeldatakse, kuidas luua hübriid pilve rakendus Microsoft Azure'i ja Visual Studio abil. Õpetuse eeldab, et te ei ole eelnevalt kogemusi, kasutades Azure. Vähem kui 30 minutit, on teil rakendus kasutab mitu Azure ressursid üles ja töötab pilveteenuses.

Saate:

-   Kuidas luua või kohandada olemasoleva web teenuse ettenähtud web lahenduse.
-   Azure'i teenus siini vahendusteenusesse abil ühiskasutusse andmine rakenduse Azure ja veebiteenuse andmete majutada mujal.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Kuidas aitab teenuse siini relay hübriid lahendused

Ärilahendused koosnevad tavaliselt kombinatsiooni kohandatud koodi kirjutada uue ja kordumatu äri nõuded ja lahenduste ja mis on juba olemasolevad funktsioonid.

Lahenduse arhitektid hakkavad kasutama pilveteenuses lihtsam käitlemine skaala nõuded ja alumise funktsionaalseid kulud. Seda tehes leiavad, et soovite kasutada koosteüksuste nende lahenduste jaoks on ettevõtte tulemüüri sisse ja välja lihtne teenuse varasid jõuda juurdepääsu pilve lahendus. Paljud sisemise teenused on ehitatud või majutatud nii, et neid saab hõlpsasti kokku ettevõtte võrgus serva.

Teenuse siini edastamine on mõeldud kasutamine juhul tegemisega olemasoleva Windows Communication Foundation (WCF) veebiteenused ja nende teenuste turvaliselt kättesaadavaks tegemine lahendusi, mis asuvad väljaspool ettevõtte perimeetri nõudmata pealetükkivad muudatusi ettevõtte võrgu taristule. Sellise teenuse siini relay teenused on endiselt majutatud oma olemasoleva keskkonnas, kuid need delegaadi listening sissetuleva meili jaoks seansid ja teenus cloud majutatud siini taotlused. Teenuse siini kaitseb ka nende teenuste volitamata juurdepääsu kaudu [Ühiskasutusse antud juurdepääs allkirja](../service-bus-messaging/service-bus-sas-overview.md) (SAS) autentimine.

## <a name="solution-scenario"></a>Lahendus stsenaarium

Selles õpetuses loote ASP.net-i veebileht, mis võimaldab teil näha toote laoseisu lehel loendi toodetest.

![][0]

Õpetuse eeldab on tooteteave olemasoleva kohapealse süsteemi ja kasutab seda süsteemi saavutamiseks teenuse siini edastamine. See on modelleerida veebiteenus, mis lihtsa konsooli rakendus töötab ja on tagatud-mälu kogumi toodete. Saab oma arvutis selle konsooli rakendus ja juurutamine web roll Azure'i sisse. Nii kuvatakse kuidas töötab Azure andmekeskuses web roll tõepoolest helistada arvutisse, kuigi teie arvuti peaaegu kindlasti hakkavad asuma vähemalt üks tulemüür ja võrgu aadress tõlge (NAT) kiht taha.

Järgnevalt on lõpule viidud veebirakenduse avalehe kuvatõmmis.

![][1]

## <a name="set-up-the-development-environment"></a>Arengu keskkonna häälestamine

Enne alustamist Azure rakenduste arendamise, tööriistad ja häälestada oma arenduskeskkond.

1.  Installige Azure'i SDK .net-i [saada tööriistad ja SDK][] lehe kaudu.

2.  Klõpsake Office'i versiooni te kasutate Visual Studio **SDK installimine** . Selle õpetuse juhised kasutada Visual Studio 2015.

4.  Kui teil palutakse käivitada või salvestada installiprogrammi, klõpsake nuppu **Käivita**.

5.  **Web platvormi Installeri**, klõpsake nuppu **Installi** ja installimist jätkata.

6.  Kui installimine on lõpule jõudnud, on teil kõik vajalik arendada rakenduse käivitamine. SDK sisaldab tööriistu, mille abil saate hõlpsalt töötada Azure rakendusi Visual Studio. Kui teil pole installitud Visual Studio, installib SDK ka tasuta Visual Studio Express.

## <a name="create-a-namespace"></a>Nimeruumi loomine

Azure'i teenus siini funktsioonide kasutamise alustamiseks tuleb esmalt luua teenuse nimeruumi. Nimeruumi pakub ulatuse container teenuse siini ressursse rakenduse tegelemiseks.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Luua kohapealse server

Esmalt koostate (fiktiivne) kohapealse toote kataloogi süsteem. See on üsna lihtne; kui tegelik asutusesisese toote kataloogi süsteemi täieliku teenuse surface, mida me proovite integreerida saavad seda vaadata.

Selle projekti Visual Studio konsooli rakendus, ja kasutab [Azure'i teenus siini Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) sisaldab teenuse siini teekide ja sätete konfigureerimine.

### <a name="create-the-project"></a>Projekti loomine

1.  Microsoft Visual Studio abil administraatoriõigused, käivitage. Visual Studio alustamiseks administraatoriõigustega Paremklõpsake **Visual Studio** programmi ikooni ja seejärel klõpsake käsku **Käivita administraatorina**.

2.  Visual Studio, klõpsake menüüs **fail** nuppu **Uus**ja valige **Project**.

3.  **Installitud Mallid**, klõpsake jaotises **Visual C#**, **Konsooli rakendus**. Tippige **nimi** väljale nimi **ProductsServer**:

    ![][11]

4.  Klõpsake nuppu **OK** , et luua **ProductsServer** projekt.

7.  Kui teil on juba installitud Nugeti package manager for Visual Studio, jätkake järgmise juhisega. Muul juhul külastage [Nugeti][] ja klõpsake nuppu [Nugeti installida](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Järgige viipasid Nugeti package manager installimiseks ja seejärel käivitage uuesti Visual Studio.

7.  Solution Exploreris Paremklõpsake **ProductsServer** projekti ja seejärel klõpsake nuppu **Halda NuGet-paketid**.

8.  Klõpsake menüüd **Sirvi** ja seejärel otsige `Microsoft Azure Service Bus`. Klõpsake nuppu **Installi**ja nõustuge kasutustingimustega.

    ![][13]

    Pange tähele, et nõutud klientrakenduse assemblereid nüüd viidatud.

9.  Lisage oma lepingu uus klass. Solution Exploreris **ProductsServer** projekti paremklõpsake ja klõpsake nuppu **Lisa**ja klõpsake **klassi**.

10. Tippige **nimi** väljale nimi **ProductsContract.cs**. Seejärel klõpsake nuppu **Lisa**.

11. Asendage **ProductsContract.cs**, järgmine kood, mis määratleb lepingu teenuse nimeruumi määratlus.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. Asendage Program.cs, järgmine kood, mis lisab kasutajaprofiili teenuse ja host seda nimeruumi määratlus.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. Topeltklõpsake Solution Exploreris **App.config** faili avamiseks Visual Studio redaktoris. Allosas olevat ** &lt;süsteem. ServiceModel&gt; ** elemendi (kuid endiselt kasutamine &lt;süsteem. ServiceModel&gt;), lisage järgmine XML-i kood. Asendage nimi oma nimeruum ja *yourKey* *yourServiceNamespace* SAS abil saate tuua varem portaalist kindlasti:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Veel App.config, klõpsake selle ** &lt;appSettings&gt; ** element, Asenda ühenduse string väärtus portaali varem saadud ühendusstring. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Vajutage **Klahvikombinatsiooni Ctrl + Shift + B** või menüüst **koostamiseks** klõpsake **Lahenduse luua** rakenduse ja kontrollima, kas teie töö seni.

## <a name="create-an-aspnet-application"></a>ASP.net-i rakenduse loomine

Selles jaotises koostate lihtne ASP.net-i rakendus, mis kuvab andmeid tuua toote teenusepakkuja.

### <a name="create-the-project"></a>Projekti loomine

1.  Veenduge, et Visual Studio töötab administraatoriõigustega.

2.  Visual Studio, klõpsake menüüs **fail** nuppu **Uus**ja valige **Project**.

3.  **Installitud Mallid**, klõpsake jaotises **Visual C#**, **ASP.net-i veebirakenduse**. Projekti **ProductsPortal**nimi. Klõpsake nuppu **OK**.

    ![][15]

4.  Klõpsake loendis **Valige mall** **MVC**. 

6.  Märkige ruut Host **pilveteenuses**.

    ![][16]

5. Klõpsake nuppu **Muuda autentimist** . Dialoogiboksis **Muuda autentimist** **Ei autentimise**nuppu ja seejärel klõpsake nuppu **OK**. Selles õpetuses juurutamist rakendus, mis ei pea kasutaja sisselogimine.

    ![][18]

6.  Dialoogiboksi **Uus ASP.net-i projekt** jaotises **Microsoft Azure'i** veenduge, et **majutada pilveteenuses** valitud ja et **Rakenduse teenus** on valitud rippmenüü loendi.

    ![][19]

7. Klõpsake nuppu **OK**. 

8. Nüüd tuleb konfigureerida Azure ressursid uue veebirakenduse. Järgige kõiki jaotise [konfigureerida Azure ressursid uue veebirakenduse](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Klõpsake selles õpetuses tagasi ja jätkake järgmise juhisega.

5.  Solution Exploreris **mudelite** paremklõpsake ja seejärel klõpsake nuppu **Lisa**, seejärel klõpsake **klassi**. Tippige **nimi** väljale nimi **Product.cs**. Seejärel klõpsake nuppu **Lisa**.

    ![][17]

### <a name="modify-the-web-application"></a>Veebirakenduse muutmine

1.  Visual Studio failis Product.cs asendage olemasolevad nimeruumi määratlus järgmine kood.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Solution Exploreris, laiendage kausta **kontrollerid** ja seejärel topeltklõpsake selle avamiseks Visual Studios **HomeController.cs** faili.

3. **HomeController.cs**, asendage olemasolevad nimeruumi määratlus järgmine kood.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Solution Exploreris laiendage Views\Shared kausta ja seejärel topeltklõpsake selle avamiseks Visual Studio redaktoris **_Layout.cshtml** .

5.  Saate muuta kõigi esinemiskordade **minu ASP.net-i rakenduse** **LITWARE's tooted**.

6. Eemaldage **Home**, **kohta**ja **kontakti** lingid. Järgmises näites Kustuta esiletõstetud kood.

    ![][41]

7.  Solution Exploreris laiendage Views\Home kausta ja seejärel topeltklõpsake selle avamiseks Visual Studio redaktoris **Index.cshtml** .
    Järgmine kood kogu sisu faili asendada.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Oma töö täpsuse, võite vajutada **Klahvikombinatsiooni Ctrl + Shift + B** projekti koostamiseks.


### <a name="run-the-app-locally"></a>Käivitage rakendus kohalikult

Käivitage rakendus kinnitamaks, et see toimib.

1.  Veenduge, et **ProductsPortal** on aktiivse projekti. Paremklõpsake Solution Exploreris projekti nime ja valige **Käivitus-projekt nimega**.
2.  Visual Studio, vajutage klahvi F5.
3.  Rakenduse peaks kuvatama, töötavad brauseris.

    ![][21]

## <a name="put-the-pieces-together"></a>Panna osad

Järgmise sammuna ühenduse luua rakenduse ASP.net-i server kohapealse tooted.

1.  Kui see pole juba avatud, Visual Studio avage see uuesti **ProductsPortal** loodud, klõpsake jaotises [Loo ASP.net-i rakenduse](#create-an-aspnet-application) .

2.  Sarnaselt etappi jaotises "Loomine an asutusesisese Server" lisamine projekti viited Nugeti pakett. Solution Exploreris Paremklõpsake **ProductsPortal** projekti ja seejärel klõpsake nuppu **Halda NuGet-paketid**.

3.  Otsige "Teenuse siini" ja **Microsoft Azure'i teenus siini** üksuse valimine. Klõpsake installimise lõpuleviimiseks ja dialoogiboksi sulgemiseks.

4.  Paremklõpsake **ProductsPortal** projekti Solution Exploreris ja seejärel klõpsake nuppu **Lisa**, seejärel **Olemasoleva kirje**.

5.  Avage fail **ProductsContract.cs** **ProductsServer** konsooli projekti kaudu. Klõpsake nuppu tõsta ProductsContract.cs. Klõpsake nuppu **Lisa**kõrval asuvat allanoolt ja seejärel klõpsake nuppu **Lisa link**.

    ![][24]

6.  Nüüd avage fail, **HomeController.cs** redaktoris Visual Studio ja asendada nimeruumi määratlus järgmine kood. Kindlasti *yourServiceNamespace* asendamine nimi oma teenuse nimeruumi ja *yourKey* SAS abil. See võimaldab kliendi helistamiseks asutusesisese teenuse, kõne tulemi esitus.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  Paremklõpsake Solution Exploreris **ProductsPortal** lahendus (Veenduge, et paremklõpsake lahenduse, mitte projekt). Klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Projekti**.

8.  Liikuge **ProductsServer** projekti ja seejärel topeltklõpsake faili **ProductsServer.csproj** lahenduse, et lisada see.

9.  **ProductsServer** peab töötama **ProductsPortal**andmeid kuvada. Lahenduste Explorer, paremklõpsake **ProductsPortal** lahendus ja klõpsake käsku **Atribuudid**. Kuvatakse dialoogiboks **Atribuutide lehtedel** .

10. Klõpsake vasakus servas **Käivitus projekti**. Klõpsake paremas servas asuvas **käivitus mitmed projektid**. Veenduge, et **ProductsServer** ja **ProductsPortal** kuvatakse, samas järjekorras koos **alustada** nii toimingu seadmine.

      ![][25]

11. Veel dialoogiboksis **Atribuudid** klõpsake **Sõltuvuste** vasakus servas.

12. Klõpsake loendis **projektide** **ProductsServer**. Veenduge, et **ProductsPortal** on **märkimata** .

14. Klõpsake loendis **projektide** **ProductsPortal**. Veenduge, et valitud oleks **ProductsServer** . 

    ![][26]

15. Klõpsake nuppu **OK** dialoogiboksis **Atribuutide lehtedel** .

## <a name="run-the-project-locally"></a>Projekti kohalik käivitamine

Rakenduse kohalik testimiseks Visual Studio vajutage **klahvi F5**. Kohapealses serveris (**ProductsServer**) peab algama esimene ja seejärel **ProductsPortal** rakendus peab algama brauseriaknas. Sel ajal, näete, et toote laoseisu loetletud andmeid tuua toote teenuse kohapealse süsteemist.

![][10]

Vajutage lehel **ProductsPortal** **värskendada** . Iga kord, kui proovite värskendada lehe, kuvatakse server rakenduse Kuva sõnum kui `GetProducts()` nimetatakse **ProductsServer** kaudu.

Sulgege mõlemad rakendused enne järgmise juhise juurde asumist.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Juurutage ProductsPortal projekt Azure web app

Järgmiseks on **ProductsPortal** frontend teisendamiseks Azure web app. Esmalt juurutada **ProductsPortal** projekt, jaotis [Deploy web projekti Azure web appi](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app)kõik juhistele. Kui juurutamise on lõpule jõudnud, õppeteema tagasi ja jätkake järgmise juhisega.

> [AZURE.NOTE] Kui **ProductsPortal** web projekti käivitatakse automaatselt pärast juurutamise, võidakse kuvada tõrketeade brauseriaknas. Seda tõenäoliselt ja põhjuseks **ProductsServer** rakendus ei tööta veel.

Kopeerige URL juurutatud veebirakenduse, vajadusel URL järgmise juhise juurde. Azure'i rakenduse tegevuse aknas Visual Studio saate hankida see URL:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Määramine ProductsPortal web app

Enne rakendus töötab pilves, peate tagama **ProductsPortal** põhjuseks jooksul Visual Studio web app.

1. Visual Studio, paremklõpsake **ProjectsPortal** projekti ja seejärel klõpsake käsku **Atribuudid**.

3. Klõpsake vasakpoolses veerus nuppu **Veeb**.

5. Jaotises **Toimingu alustamiseks** klõpsake nuppu **Käivita URL-i** ja teksti väljale URL-i varem juurutatud veebiversiooni; näiteks `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Visual Studio menüü **fail** käsku **Salvesta kõik**.

7. Klõpsake Visual Studio Build menüü **Lahenduse taastada**.

## <a name="run-the-application"></a>Käivitage rakendus

2.  Vajutage klahvi F5 koostamine ja käivitage rakendus. Kohapealses serveris ( **ProductsServer** konsooli rakendus) peab algama esimene ja seejärel **ProductsPortal** rakendus peab algama brauseriaknas, nagu on näidatud järgmisel kuvatõmmisel kuvatõmmis. Märkate uuesti, et toote varude andmeid tuua toote teenuse kohapealse süsteemist loendid ja kuvab andmeid web Appis. Veenduge, et **ProductsPortal** töötab pilves, mis Azure web appi URL-i kontroll 

    ![][1]

    > [AZURE.IMPORTANT] **ProductsServer** konsooli rakendus peab olema töötama ning teenida **ProductsPortal** rakenduse andmeid. Kui brauseris kuvatakse tõrge, oodake veel paar sekundit **ProductsServer** laadimise ja kuvada järgmine teade. Vajutage **värskendamine** brauseris.

    ![][37]

3. Tagasi brauseris, vajutage **värskendamine** **ProductsPortal** lehe. Iga kord, kui proovite värskendada lehe, kuvatakse server rakenduse Kuva sõnum kui `GetProducts()` nimetatakse **ProductsServer** kaudu.

    ![][38]

## <a name="next-steps"></a>Järgmised sammud  

Lisateavet teenuse siini, leiate järgmistest teemadest:  

* [Azure'i teenus siini][sbwacom]  
* [Kuidas kasutada teenuse siini järjekorrad][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Tööriistad ja SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [Nugeti]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

