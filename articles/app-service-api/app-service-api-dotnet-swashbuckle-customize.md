
<properties 
    pageTitle="Swashbuckle genereeritud API määratluste kohandamine" 
    description="Saate teada, kuidas kohandada ärplema API määratlused loodud Swashbuckle API rakenduse teenuses Azure rakendus." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Swashbuckle genereeritud API määratluste kohandamine 

## <a name="overview"></a>Ülevaade

Selles artiklis selgitatakse, kuidas kohandada Swashbuckle käsitlema Levinumad stsenaariumid, kus võite soovida muuta vaikekäitumine:

* Swashbuckle genereeritud dubleeritud toiming identifikaatoreid ülekoormuse kontrolleril võimalused
* Swashbuckle eeldab, et ainult kehtiv vastus meetod on HTTP 200 (OK) 
 
## <a name="customize-operation-identifier-generation"></a>Toiming identifikaator genereerimine kohandamine

Swashbuckle loob ärplema toiming identifikaatorite, ühendades domeenikontrolleri nimi ja meetodi nime. See muster loob probleem, kui teil on mitu ülekoormuse meetodi: Swashbuckle genereeritud dubleeritud toimingu ID-d, mis on sobimatu ärplema JSON.

Näiteks järgmine kood kontrolleril põhjustab Swashbuckle kolme Contact_Get toiming ID-sid luua.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Saate lahendada probleemi käsitsi, andes meetodite kordumatute nimede panemine, nagu näiteks järgmine:

* Hankimine
* GetById
* GetPage

Teise võimalusena on laiendada Swashbuckle teha seda automaatselt genereerida toiming kordumatu ID-d.

Järgmised toimingud näitab, kuidas kohandada Swashbuckle *SwaggerConfig.cs* faili, mis on kaasatud projekti Visual Studio API rakenduste eelvaate projekti malli abil.  Samuti saate kohandada Swashbuckle Veebiteenuste Projectis, mis rakenduse API juurutamiseks konfigureerida.

1. Luua kohandatud `IOperationFilter` rakendamine 

    Funktsiooni `IOperationFilter` kasutajaliidese pakub Swashbuckle kasutajatele, kes soovivad kohandada eri omadusi ärplema metaandmete protsess on laiendatavus punkt. Järgmine kood näitab üks meetod toiming-id-genereerimine käitumise muutmine. Koodi lisab parameetrite nimed toimingu id nimi.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. *App_Start\SwaggerConfig.cs* failis, kõne on `OperationFilter` meetodit kasutada uut Swashbuckle põhjustada `IOperationFilter` rakendamist.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    *SwaggerConfig.cs* fail, mille katkeb Swashbuckle Nugeti pakett sisaldab palju välja kommenteeritud näiteid laiendatavuse punkte. Täiendavate kommentaaride on näidatud siin. 

    Pärast selle muudatuse tegemist oma `IOperationFilter` rakendamine kasutatakse ja põhjustab genereeritakse kordumatu toimingu ID-d.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Luba vastuse koodide 200 peale

Vaikimisi Swashbuckle eeldab, et vastuse HTTP 200 (OK) on *ainult* õigustatud vastuse Web API-meetod. Mõnel juhul võite põhjustada erandi tõsta kliendi ilma muude vastuse koodide tagastamiseks.  Näiteks järgmine kood Veebiteenuste näitab, kus soovite kliendi aktsepteerimiseks 200 või nimega kehtiv vastused 404 stsenaarium.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

Selle stsenaariumi korral ärplema, mis Swashbuckle loob vaikimisi määrab ainult üks korralikud HTTP olekukoodi, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Kuna Visual Studio kasutab ärplema API määratluse genereerida koodi klient, loob see tõstab erandi mis tahes vastus peale mõne HTTP 200 kliendi tähist. Allpool kood on C# klientrakenduses loodud see valimi Veebiteenuste meetod.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle pakub kaks võimalust kohandamise oodatud HTTP vastuse koodide loodava, loetelu XML-i kommentaaride abil või `SwaggerResponse` atribuut. Atribuut on lihtsam, kuid see on ainult saadaval Swashbuckle 5.1.5 või uuem versioon. Visual Studio 2013 API Apps preview uue projekti Mall sisaldab Swashbuckle versioon 5.0.0, nii, et kui kasutasite Mall ja ei soovi Swashbuckle värskendada, ainult on kasutada XML-i kommentaarid. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>XML-i kommentaaride abil oodata vastust koodide kohandamine

Selle meetodi abil saate määrata vastuse koodide teie Swashbuckle versioon on varasem kui 5.1.5.

1. XML-i dokumentatsiooni kommentaare lisada esmalt üle meetodid, mida soovite määrata HTTP vastuse koodid. Vastamist valimi Veebiteenuste toimingu eespool näidatud ja XML-i dokumentatsiooni rakendamine tooks kood nagu järgmises näites. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Lisa juhiseid *SwaggerConfig.cs* faili suunamiseks kasutada XML-i Swashbuckle dokumentatsioon faili.

    * Avage *SwaggerConfig.cs* ja luua meetodi *SwaggerConfig* klassi määramiseks dokumentatsiooni XML-faili tee. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Liikuge kerides allapoole *SwaggerConfig.cs* faili seni, kuni näete kommenteeritud kontorist rida koodi, mis sarnaneb allpool ekraanipilt. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Eemalda kommentaar rea lubamiseks töötlemise ajal ärplema genereerimine XML-i kommentaarid. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Dokumentatsiooni XML-faili loomiseks, lähevad projekti atribuudid ja lubage XML-dokumentatsioon faili, nagu on näidatud alloleval pildil. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Pärast nende juhiste täitmiseks loodud Swashbuckle ärplema JSON kajastuvad HTTP vastuse koodide teie määratud XML-i kommentaarid. Pildil näitab selle uue JSON last. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Visual Studio abil saate taastada oma REST API kliendi kood, aktsepteerib C# koodi nii HTTP OK kui ka ei leitud olek koodide ilma tõstmine erandi lubamisel oma nõudvate koodi otsuste null kontakti kirje tagasi käsitsemise kohta. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

See näide kood leiate [selle GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse)hoidla. Koos veebi-API on märgitud XML-i dokumentatsiooni kommentaaridega projekti konsooli rakendus projekti, mis sisaldab seda API loodud kliendi. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Oodatud vastuse koodide kasutamine SwaggerResponse atribuut kohandamine

Atribuut [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) on saadaval Swashbuckle 5.1.5 ja uuemad versioonid. Juhuks, kui teil on mõni varasem versioon projektis, käivitab selles jaotises selgitatakse, kuidas värskendada, et saate selle atribuudi Swashbuckle Nugeti pakett.

1. **Solution Exploreris**paremklõpsake Web API projekti ja klõpsake nuppu **Halda NuGet-paketid**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. *Swashbuckle* Nugeti pakett kõrval nuppu *Värskenda* . 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. Saate lisada Veebiteenuste toiming meetodite jaoks, mille soovite määrata lubatud HTTP vastuse koodide *SwaggerResponse* atribuute. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Lisada mõne `using` on atribuut nimeruum-lause:

        using Swashbuckle.Swagger.Annotations;
        
1. Liikuge sirvides oma projekti */swagger/docs/v1* URL-i ja erinevate HTTP vastuse koodid on nähtav ärplema JSON. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

See näide kood leiate [selle GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes)hoidla. Veebi-API koos projekti kujundatud *SwaggerResponse* atribuut on konsooli rakendus projekti, mis sisaldab loodud kliendi selle API. 

## <a name="next-steps"></a>Järgmised sammud

Selles artiklis on näidatud nii, nagu Swashbuckle loob toiming ID-d ja kehtiv vastuse koodide kohandamiseks. Lisateavet leiate teemast [Swashbuckle github](https://github.com/domaindrivendev/Swashbuckle).
 
