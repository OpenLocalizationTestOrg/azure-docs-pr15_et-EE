<properties
   pageTitle="Teenuse põhisumma portaali loomine | Microsoft Azure'i"
   description="Kirjeldab, kuidas luua uus Active Directory rakendus ja teenuse põhilise, mida saab kasutada koos Rollipõhine juurdepääsu reguleerimine Azure'i ressursihaldur ressursid juurdepääsu haldamiseks."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Portaali abil saate luua Active Directory rakenduste ja teenuste põhisumma, millele pääsete juurde ressursid

> [AZURE.SELECTOR]
- [PowerShelli](resource-group-authenticate-service-principal.md)
- [Azure'i CLI](resource-group-authenticate-service-principal-cli.md)
- [Portaal](resource-group-create-service-principal-portal.md)


Kui teil on rakendus, mis tuleb avada või muuta ressursse, saate häälestada rakenduse Active Directory (AD) ja vajalike õiguste määramine. See teema näitab, kuidas nende toimingute portaali kaudu. Praegu, peate kasutama klassikaline portaali luua uue Active Directory rakenduse, ja seejärel vahetage Azure portaali rakenduse rollile määrata. 

> [AZURE.NOTE] Selles teemas juhised kehtivad ainult **klassikaline portaali** loomine AD rakenduse kasutamisel. **Kui kasutate Azure portaali loomise rakendus AD, ei õnnestu neid juhiseid.** 
>
> Teil võib-olla lihtsam häälestada oma AD rakenduste ja teenuste põhisumma [PowerShelli](resource-group-authenticate-service-principal.md) või [Azure CLI](resource-group-authenticate-service-principal-cli.md), eriti siis, kui soovite kasutada serdi autentimist. See teema ei kuvata serdi kasutamise kohta.

Active Directory mõisted selgitused, leiate teemast [rakenduse objektid ja teenuse põhilise objektidele](./active-directory/active-directory-application-objects.md). Active Directory autentimise kohta leiate lisateavet teemast [Azure AD autentimise stsenaariumid](./active-directory/active-directory-authentication-scenarios.md).

Üksikasjalikud juhised rakenduse integreerimine Azure ressursside haldamise kohta leiate teemast [arendaja juhend autoriseerimine Azure'i ressursihaldur API-ga](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Active Directory rakenduse loomine

1. Logige sisse oma Azure'i kontosse [klassikaline portaali](https://manage.windowsazure.com/)kaudu. 

2. Veenduge, et teate tellimuse jaoks vaikesätteid Active Directory. Saate anda ainult Accessi rakenduste tellimuse samas kataloogis. Valige **sätted** ja vaadake, kas teie tellimusega seostatud kausta nimi.  Lisateabe saamiseks lugege teemat [Kuidas Azure'i tellimused on seostatud Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![vaikekataloogi otsimine](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. Valige vasakul paanil **Active Directory** .

     ![Valige Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Valige Active Directory, mida soovite kasutada loomise rakendus. Kui teil on rohkem kui üks Active Directory, luua rakenduse vaikekataloogi tellimuse.   

     ![Valige kataloog](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Rakenduste kataloogi vaatamiseks valige **rakendused**.

     ![Kuva rakendused](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Kui te pole loonud rakenduse enne selle kausta, peaksite nägema midagi sarnast järgmisel pildil. Valige **rakenduse lisamine**

     ![rakenduse lisamine](./media/resource-group-create-service-principal-portal/create-application.png)

     Või klõpsake käsku **Lisa** alumise paani.

     ![lisamine](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Valige rakendus, mida soovite luua. Valige selles õpetuses **ettevõttesiseselt areneb rakenduse lisamine**. 

     ![Uus rakendus](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Rakenduse nimi ja valige rakendus, mida soovite luua. Selles õpetuses soovitud **Rakendus ja/või WEB Veebiteenuste** loomine ja klõpsake nuppu edasi. Kui valite **Kohalikke KLIENTRAKENDUSEGA**, selle artikli juhiseid ei sobi teie töötamist.

     ![rakenduse nimi](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Sisestage oma rakenduse atribuudid. **Logi-ON URL-i**, sisestage URI veebisaidile, mis kirjeldab rakenduse. Veebisaidi olemasolu ei kontrollita. **Rakenduse ID URI**, esitage URI, mille abil tuvastatakse teie rakendus.

     ![Teenuserakenduse atribuutide](./media/resource-group-create-service-principal-portal/app-properties.png)

Olete loonud rakenduse.

## <a name="get-client-id-and-authentication-key"></a>Saada kliendi id ja autentimise võti

Kui programmiliselt logimine, peate rakenduse ID-d. Kui rakendus töötab vastavalt oma mandaat, samuti vajate mõne autentimise võti.

1. Valige vahekaart **konfigureerimine** konfigureerida rakenduse parool.

     ![rakenduse konfigureerimine](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopeerige **Kliendi ID**.
  
     ![kliendi id](./media/resource-group-create-service-principal-portal/client-id.png)

3. Kui rakendus töötab vastavalt oma mandaat, liikuge kerides jaotiseni **võtmed** ja valige, kui kaua soovite parool on lubatud.

     ![Kiirklahvid](./media/resource-group-create-service-principal-portal/create-key.png)

4. Valige **Salvesta** võtme loomiseks.

     ![salvestamine](./media/resource-group-create-service-principal-portal/save-icon.png)

     Salvestatud võti kuvatakse ja saate selle kopeerida. Te ei saa võti hiljem kuulata nii kopeerige see kohe.

     ![Salvestatud võti](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Rentniku id hankimine

Sisselogimisel programmiliselt peate läbima rentniku id autentimine taotlusele. Veebirakenduste ja API veebirakenduste, saate tuua rentniku id, valides ekraani allservas **Vaade lõpp-punktid** ja toomine id, nagu on näidatud järgmisel pildil.  

   ![rentniku id](./media/resource-group-create-service-principal-portal/save-tenant.png)

Saate kuulata ka rentniku id PowerShelli kaudu:

    Get-AzureRmSubscription

Või Azure CLI:

    azure account show --json

## <a name="set-delegated-permissions"></a>Delegeeritud õiguste määramine

Kui teie rakendus kasutab ressursse sisselogitud kasutaja nimel, tuleb tagada rakenduse muudes rakendustes delegeeritud juurdepääsuõigust. **Konfigureerige** vahekaardil **õigused muude rakenduste** osas juurdepääsu andmine Vaikimisi on juba delegeeritud õiguste lubatud Azure Active Directory jaoks. Jätke see delegeeritud õigus muutmata.

1. Valige **Lisa rakendus**.

2. Valige loendist **Windows Azure'i teenuse juhtimise API**. Seejärel valige ikooni valmis.

      ![Valige rakendus](./media/resource-group-create-service-principal-portal/select-app.png)

3. Valige ripploendis delegeeritud õiguste **Juurdepääsu Azure Teenusehaldus ettevõtte nimega**.

      ![Valige õigus](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Muudatuse salvestamiseks.

## <a name="assign-application-to-role"></a>Rakenduse rolli määramine

Kui teie rakendus töötab vastavalt oma mandaat, peate määrama rollile rakendus. Otsustada, millist rolli tähistab rakenduse asjakohased õigused. Saadaval rollide kohta leiate teemast [RBAC: sisseehitatud rollid](./active-directory/role-based-access-built-in-roles.md). 

Rakenduse rolli määramiseks peab teil vajalikke õigusi. Täpsemalt, peab teil olema `Microsoft.Authorization/*/Write` antakse rolli [omanik](./active-directory/role-based-access-built-in-roles.md#owner) või administraatorirolli ja [kasutajale](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) kaudu. Kaasautori roll pole õige juurdepääsu.

Saate seada ulatuse tasemel tellimus, ressursirühm või ressurss. Õigused päritakse madalama taseme ulatus. Näiteks lisage lugeja rollile ressursirühma rakendus, tähendab see saab lugeda ressursirühma ja see sisaldab ressursse.

1. Rakenduse lisamine rolli määramine, aktiveerige klassikaline portaalis [Azure portaali](https://portal.azure.com).

1. Kontrollige, veendumaks, et teenuse põhilise saate määrata rolli õigused. Valige **minu õigused** teie konto jaoks.

    ![Valige minu õigused](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Teie konto jaoks määratud õiguste vaatamine. Nagu varem, peate kuuluvad omanik või kasutaja juurdepääs administraatori rollid, või kohandatud rolli, mis annab Microsoft.Authorization kirjutamisõigused. Järgmisel pildil on kujutatud kaasautori roll tellimuse, mis ei ole piisavaid õigusi määrata rakenduse rollile määratud konto.

    ![Kuva minu õigused](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Kui teil pole vajalikke õigusi anda rakenduse juurdepääsu, peate kas taotluse, et teie tellimus administraator teid lisab administraatorirolli ja kasutajale või koosolekukutse, et administraator annab juurdepääsu rakenduse.

1. Liikuge ulatus, mille soovite määrata rakenduse tase. Mõne rolli tellimuse ulatuse määramiseks valige **tellimuste**.

     ![Valige tellimus](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Valige rakendus, et määrata teatud tellimus.

     ![Valige tellimus, ülesande](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     Valige paremas ülanurgas ikooni **juurdepääsu** .

     ![Valige juurdepääs](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Või ressursside rühma ulatuses rolli määramine, liikuge ressursirühma. Valige keelest ressursi rühma **juurdepääsu reguleerimine**.

     ![Valige kasutajad](./media/resource-group-create-service-principal-portal/select-users.png)

     Järgmised toimingud on sama mis tahes ulatust.

2. Valige **Lisa**.

     ![Valige lisa](./media/resource-group-create-service-principal-portal/select-add.png)

3. Valige roll, **lugejale** (või mis tahes rolli soovite määrata rakenduse).

     ![Valige roll](./media/resource-group-create-service-principal-portal/select-role.png)

4. Saate lisada rollile kasutajate loendi esmakordsel kuvamisel kuvatakse rakendused. Kuvatakse ainult rühma ja kasutajad.

     ![kasutajate kuvamine](./media/resource-group-create-service-principal-portal/show-users.png)

5. Rakenduse leidmiseks saate otsida seda. Hakake tippima oma rakenduse nimi ja muudab saadaolevate suvandite loend. Kui näete loendis, valige oma rakendus.

     ![rolli määramine](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Valige **Okay** lõpuleviimiseks rolli määramine. Nüüd näete rakenduse kasutab ressursirühma rollile määratud loendis.


Kasutajate ja rollide portaali kaudu rakenduste määramise kohta leiate lisateavet teemast [kasutamine rollimääranguid juurdepääsu oma ressursse Azure tellimuse haldamiseks](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## <a name="sample-applications"></a>Lihtsad rakendused

Kuidas sisse logida teenuse peamise valimi järgmiste rakenduste kuvamine

**.NET-I**

- [Juurutamine on SSH lubatud VM malli .net-i abil](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure'i ressursse ja .net-i ressursikeskuse rühmade haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Java kasutamise alustamine – juurutada Azure ressursihaldur malli abil - ressursid](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Java ressursse - ressursside rühma haldamine – alustamine](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Juurutamine on SSH lubatud VM Python malli abil](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Azure'i ressursside ja ressursside rühmade Python haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Juurutamine on SSH lubatud VM Node.js malli abil](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure'i ressursid ja ressursside Node.js rühmade haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Juurutamine on SSH lubatud VM Ruby malli abil](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Azure'i ressursside ja ressursside Ruby rühmade haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Järgmised sammud

- Turbepoliitikate määramise kohta leiate teemast [Azure Rollipõhine juurdepääsu reguleerimine](./active-directory/role-based-access-control-configure.md).  
- Tutvustava video neid juhiseid, vaadake [Lubamine programmiline haldamise Azure Active Directory on Azure ressurss](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

