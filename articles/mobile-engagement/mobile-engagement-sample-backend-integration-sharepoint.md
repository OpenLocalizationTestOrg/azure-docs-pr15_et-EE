<properties 
    pageTitle="Azure'i mobiilsideseadmete kaasamine - taustväärtus integreerimine" 
    description="Azure'i Mobile kaasamine kasutajaga SharePointi kirjutamata loomiseks kampaaniat SharePointist" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure'i mobiilsideseadmete kaasamine - API integreerimine

Automaatse turundustegevuse süsteemi, loomine ja selle turunduskampaaniate aktiveerimine ka tekkida automaatselt. Selleks - Azure Mobile kaasamine võimaldab sellise automatiseeritud turunduskampaaniate abil ka API-de loomiseks. 

Tavaliselt kliendid Mobile kaasamine esiosa kasutajaliidese abil luua oma turunduskampaaniate osana teadaanded/küsitluste jne. Siiski soovitud turunduskampaaniate muutuvad küps, on vaja ära kasutada andmete lukustatud taustväärtus süsteemide (nt mis tahes CRM süsteemi või CMS süsteem nagu SharePoint), et täielikult automatiseeritud müügivõimaluste saab luua, mis loob kampaaniat Mobile kaasamine dünaamiliselt taustväärtus süsteemide rakenduses tulenevad andmete põhjal. 

![][5]

Selle õpetuse läheb läbi stsenaarium, kus SharePointi ärikasutaja kuvab turundusmaterjalide andmetega SharePointi loendi ja automaatse protsessi jätkab loendi üksused ja ühendab Mobile kaasamine süsteemi saadaval REST API-de abil turunduskampaania loomine SharePointi andmete põhjal. 
 
> [AZURE.IMPORTANT] Üldiselt, saate selle valimi lähtepunktina mõista, kuidas kutsuda mis tahes Mobile kaasamine REST API, nagu see üksikasjad kahe võtme aspektide helistamiseks API - autentiva ja läheb parameetrid. 

## <a name="sharepoint-integration"></a>SharePointi integreerimine
1. Siin on valimi SharePointi loendi välja näeb. **Pealkiri**, **kategooria**, **NotificationTitle**, **sõnumi** ja **URL-i** kasutatakse väljakuulutamist luua. On veerg nimega **IsProcessed** kasutab valimi automatiseerimise protsess konsooli programmi kujul. Saate kas Käivita see konsool programm on Azure WebJob nimega nii, et saaksite seda plaanida või otse abil saate SharePointi töövoo loomise ja aktiveerimise väljakuulutamist, kui üksus on SharePointi loendisse lisatud programm. Selles näites me kasutame konsooli programm, mis läheb läbi SharePointi loendi üksuste ja loomine teadaanne Azure Mobile kaasamine neid ja seejärel Lõpuks märgib **IsProcessed** lipp peab olema täidetud eduka teadaanne loomine.

    ![][1]

2. Kasutame *Serveri autentimine SharePoint Online kliendi objektimudeli abil* proovi kood [siin](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) autentida SharePointi loendiga.
 
3. Pärast autentimist, saame pidev abil välja selgitada, mis tahes äsja loodud üksuste loendi üksuste (mis on **IsProcessed** = false). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Integratsiooni Mobile kaasamine
1.  Kui me leidnud üksuse, mille jaoks on vaja töötlemine - me ekstrakti loendiüksuse ja kõne teate loomiseks vajaliku teabe `CreateAzMECampaign` selle loomiseks ja seejärel `ActivateAzMECampaign` selle aktiveerimiseks. Need on põhiosas REST API kõned kutsudes Mobile kaasamine kirjutamata. 

2.  Mobile kaasamine REST API-de nõua **Basic auth värviskeemi autoriseerimine HTTP päise** , mis koosneb selle `ApplicationId` ja `ApiKey` , mis kuvatakse Azure portaalist. Veenduge, et kasutate võti **api klahvid** jaotises ja *pole* jaotise **sdk võtmed** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Väljakuulutamist loomiseks tippige turunduskampaania – vaadake [dokumentatsiooni](https://msdn.microsoft.com/library/azure/mt683750.aspx). Veenduge, et määrate kampaaniat peate `kind` *teadaanne* ja [last](https://msdn.microsoft.com/library/azure/mt683751.aspx) ja saata see nimega FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Kui olete teadet, mis on loodud, kuvatakse umbes järgmine Mobile kaasamine portaali (Pange tähele, et olek = mustand ja aktiveeritud = N/A)

    ![][3]

5. `CreateAzMECampaign`loob mõne teadaanne turunduskampaania ja tagastab selle Id funktsiooni helistaja. `ActivateAzMECampaign`nõuab selle Id parameetrina kampaaniat aktiveerimiseks. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Kui teil on aktiveeritud väljakuulutamist, kuvatakse umbes järgmine Mobile kaasamine portaalis.

    ![][4]

7. Kohe pärast kampaaniat saab aktiveeritud, hakkavad mis tahes seadmed, mis vastavad kriteeriumile kampaaniat kuvatakse teatised. 

8. Märkate, et loendiüksus tähistatud IsProcessed = false seatud väärtusele tõene, kui teadaanne turunduskampaania on loodud.  

Selle valimi luua lihtsa teadaanne kampaaniat enamasti nõutavate atribuutide. Saate kohandada see nii palju, kui portaalis toodud teabe abil saate [siin](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
