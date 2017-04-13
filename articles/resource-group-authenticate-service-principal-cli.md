<properties
   pageTitle="Luua teenuse põhisumma Azure'i CLI | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada Azure CLI Active Directory rakenduste ja teenuste põhisumma loomine ja anda selle juurdepääsu abil Rollipõhine juurdepääsu reguleerimine. See näitab, kuidas rakenduse parooli või serdiga autentida."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Azure'i CLI abil saate luua teenuse põhilise juurdepääs ressurssidele

> [AZURE.SELECTOR]
- [PowerShelli](resource-group-authenticate-service-principal.md)
- [Azure'i CLI](resource-group-authenticate-service-principal-cli.md)
- [Portaal](resource-group-create-service-principal-portal.md)


Kui teil on rakenduse või skripti, mida vajab ressursse, siis tõenäoliselt soovite käivitada oma mandaadi alusel importimistoimingut. Teil võib olla teistsugused õigused, mida soovite rakenduse ja te ei soovi rakenduse edasi kasutada oma kasutajanimi ja parool, kui teie kohustused muuta. Selle asemel saate luua rakenduse, mis sisaldab autentimiseks mandaadi ja rollimääranguid identiteedi. Iga kord, kui rakendus töötab, see autendib ise neid mandaate. Selles teemas näidatakse, kuidas [Mac, Linux ja Windows Azure'i CLI](xplat-cli-install.md) abil häälestada rakendus oma mandaadi ja identiteedi käivituma.

Azure'i CLI, on teil kaks võimalust autentimiseks AD Rakenduse:

 - parooli
 - sert

Selles teemas kirjeldatakse, kuidas kasutada Azure CLI mõlema variandi. Kui kavatsete jätta programmide (nt Python, Ruby või Node.js) Azure'i sisse logida, võib Paroolautentimine oma parim valik. Otsustada, kas kasutada parooli või serti jaotisest [valimi rakenduste](#sample-applications) näiteid erinevate raamistikku autentimist.

## <a name="active-directory-concepts"></a>Active Directory mõisted

Selles artiklis saate luua kaks objekti – Active Directory (AD) rakenduste ja teenuste põhisumma. AD rakendus on globaalne kujutis rakenduse. See sisaldab identimisteabe (rakenduse id ja parool või sert). Peamine teenus on kohalik esindus rakenduse Active Directory. See sisaldab rolli määramine. Selles teemas käsitletakse ühe – rentniku rakenduse, kui rakendus on mõeldud ainult üks ettevõttes käivitada. Tavaliselt ühe – rentniku rakenduste jaoks kasutatavasse ärivaldkonna rakendusi, mis töötavad teie ettevõttes. Ühe-rentniku rakenduses, on üks AD Rakenduse ja teenuse ühe põhilise.

Võib tekkida – miks on vaja nii objektid? Seda moodust mõttekas rohkem kui mõelda mitme rentniku rakendused. Tavaliselt kasutate mitme rentniku rakenduste tarkvara-kui-a-service (SaaS) rakenduste, kus teie rakendus töötab paljudes erinevates tellimustes. Mitme rentniku rakenduste, on üks AD rakendus ja mitme teenuse põhisumma (üks iga Active Directory, mis annab rakendus). Mitme rentniku rakenduse vaadake [arendaja juhend autoriseerimine Azure'i ressursihaldur API-ga](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Vajalikke õigusi

Selles teemas lõpuleviimiseks peab teil olema nii teie Azure Active Directory ja Azure tellimuse piisavaid õigusi. Täpsemalt, peate olema võimalus luua rakenduse Active Directory ja teenuse põhilise rollile määrata. 

Teie Active Directory konto peab olema administraator (nt **Üldadministraatori** või **Kasutaja haldus**). Kui teie konto on määratud **kasutajale** , peate olema administraator tõsta teie õigused.

Teie tellimus on teie konto peab olema `Microsoft.Authorization/*/Write` antakse rolli [omanik](./active-directory/role-based-access-built-in-roles.md#owner) või administraatorirolli ja [kasutajale](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) kaudu juurdepääsu. Kui teie konto on määratud **kaasautori** roll, saate tõrketeate, kui proovite teenuse põhilise rollile määrata. Uuesti oma tellimuse administraator peab teile andma piisavalt juurdepääsu.

Nüüd jätkake jaotise kas [parooli](#create-service-principal-with-password) või [serdi](#create-service-principal-with-certificate) autentimine.

## <a name="create-service-principal-with-password"></a>Teenuse parooliga põhisumma loomine

Selle jaotise parooliga AD Rakenduse loomiseks tehke ja põhisumma teenusega lugeja rolli määramine.

Vaatame järgmist.

1. Logige sisse oma konto.

        azure login

1. On teil kaks võimalust AD rakenduste loomise kohta. Saate luua AD rakenduste ja teenuste põhisumma ühe sammu või luua neid eraldi. Luua neid sammhaaval, kui teil pole vaja määrata avalehe ja oma rakenduse identifikaator URI-d. Luua neid eraldi, kui teil on vaja need väärtused web appi. Selles etapis tuleb kuvatakse mõlema variandi.

     - Luua AD rakenduste ja teenuste põhisumma ühe sammu, sisestage nimi rakenduse ja parooli, nagu on näidatud järgmine käsk:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Eraldi AD Rakenduse loomiseks sisestage nimi rakenduse, avalehe URI, identifikaator URI-d ja parooli, nagu on näidatud järgmine käsk:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Eelmise käsu tagastab AppId väärtus. Teenuse põhilise loomiseks sisestage selle väärtuse parameetrina järgmine käsk:
     
            azure ad sp create -a <AppId>
     
     Kui teie kontol puuduvad [vajalikud õigused](#required-permissions) Active Directory, kuvatakse tõrketeade, mis näitab "Authentication_Unauthorized" või "kontekstis tellimust ei leitud".
    
     Mõlema variandi, tagastatakse uue teenuse põhilise. **Objekti Id** on vaja õiguste andmiseks. **Teenuse subjektinimede** loetletud guid on vaja sisselogimisel. See guid on sama väärtuse, mis rakenduse ID-d. Rakendustes valimi selle väärtuseks on edaspidi **Kliendi ID**. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Teenuse põhisumma õiguste tellimuse andmine. Selles näites saate lisada teenuse põhilise **lugeja** rolli, mis annab õiguse lugeda kõik ressursid tellimuse. Muud rollid, lugege teemat [RBAC: sisseehitatud rollid](./active-directory/role-based-access-built-in-roles.md). **Andke** parameetri ette **ObjectId väärtuse** , mida kasutasite rakenduse loomisel. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Kui teie konto pole piisavaid õigusi määrata rolli, kuvatakse tõrketeade. Teade teie konto **on luba Microsoft.Authorization/roleAssignments/write' üle ulatus "/ tellimuste / {guid}" toimingu sooritamiseks**. 

See on õige! Oma AD rakenduste ja teenuste põhisumma häälestada. Järgmises jaotises kirjeldatakse, kuidas sisse logima mandaati Azure'i CLI kaudu. Kui soovite koodi rakenduse kasutamine mandaat, peate selle teema jätkata. Saate liikuda [valimi rakenduste](#sample-applications) näiteid oma rakenduse id ja parooliga sisse logimine. 

### <a name="provide-credentials-through-azure-cli"></a>Mandaat Azure'i CLI kaudu

Nüüd, peate sisse logima rakenduse toimingute tegemiseks.

1. Iga kord, kui logite sisse teenuse põhi, peate kataloogi rentniku ID-d pakuvad AD Rakenduse. Rentniku on Active Directory eksemplar. Praegu autenditud tellimuse id rentniku toomiseks kasutada:

        azure account show

     Mis annab:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Kui vajate mõne muu tellimuse rentniku ID-d, kasutage järgmist käsku:

        azure account show -s {subscription-id}

2. Kui teil on vaja tuua kliendi id logimiseks kasutada, kasutage.

        azure ad sp show -c exampleapp --json

     Sisselogimiseks kasutatava väärtuse on loetletud teenuse põhilist nime guid.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Logige sisse teenuse põhilise.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    Küsitakse parooli. Sisestage parool määratud AD Rakenduse loomisel.

        info:    Executing command login
        Password: ********

Nüüd on kinnitatud, nagu teenuse peamine jaoks teenuse põhilise loodud.

## <a name="create-service-principal-with-certificate"></a>Teenuse serdiga põhisumma loomine

Selle jaotise juhiste abil:

- iseallkirjastatud serdi loomine
- serdi ja teenuse põhilise AD Rakenduse loomine
- teenuse põhilise lugeja rolli määramine

Nende juhiste täitmiseks peab teil olema [OpenSSL](http://www.openssl.org/) installitud.

1. Iseallkirjastatud serdi loomine.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Ühendada avalike ja privaatvõ võtmed.

        cat privkey.pem cert.pem > examplecert.pem

3. **Examplecert.pem** faili avada ja otsige märkide vahel **---Alguskuupäev serdi---** ja **---lõpetamine serdi---**pikk jada. Kopeerige andmed sert. Te kaotate andmed parameetrina loomisel teenuse põhisumma.

1. Logige sisse oma konto.

        azure login

1. On teil kaks võimalust AD rakenduste loomise kohta. Saate luua AD rakenduste ja teenuste põhisumma ühe sammu või luua neid eraldi. Luua neid sammhaaval, kui teil pole vaja määrata avalehe ja oma rakenduse identifikaator URI-d. Luua neid eraldi, kui teil on vaja need väärtused web appi. Selles etapis tuleb kuvatakse mõlema variandi.

     - Luua AD rakenduste ja teenuste ühe sammu põhisumma, sisestage nimi rakenduse ja serdi andmed, nagu on näidatud järgmine käsk:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Eraldi AD Rakenduse loomiseks sisestage nimi rakenduse, avalehe URI, identifikaator URI-d ja serdi andmed, nagu on näidatud järgmine käsk:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Eelmise käsu tagastab AppId väärtus. Teenuse põhilise loomiseks sisestage selle väärtuse parameetrina järgmine käsk:
     
            azure ad sp create -a <AppId>
  
     Kui teie kontol puuduvad [vajalikud õigused](#required-permissions) Active Directory, kuvatakse tõrketeade, mis näitab "Authentication_Unauthorized" või "kontekstis tellimust ei leitud".
    
     Mõlema variandi, tagastatakse uue teenuse põhilise. Objekti Id on vaja õiguste andmiseks. **Teenuse subjektinimede** loetletud guid on vaja sisselogimisel. See guid on sama väärtuse, mis rakenduse ID-d. Rakendustes valimi selle väärtuseks on edaspidi **Kliendi ID**. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Teenuse põhisumma õiguste tellimuse andmine. Selles näites saate lisada teenuse põhilise **lugeja** rolli, mis annab õiguse lugeda kõik ressursid tellimuse. Muud rollid, lugege teemat [RBAC: sisseehitatud rollid](./active-directory/role-based-access-built-in-roles.md). **Andke** parameetri ette **ObjectId väärtuse** , mida kasutasite rakenduse loomisel. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Kui teie konto pole piisavaid õigusi määrata rolli, kuvatakse tõrketeade. Teade teie konto **on luba Microsoft.Authorization/roleAssignments/write' üle ulatus "/ tellimuste / {guid}" toimingu sooritamiseks**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Sisestage serdi automatiseeritud Azure'i CLI skripti kaudu

Nüüd, peate sisse logima rakenduse toimingute tegemiseks.

1. Iga kord, kui logite sisse teenuse põhi, peate kataloogi rentniku ID-d pakuvad AD Rakenduse. Rentniku on Active Directory eksemplar. Praegu autenditud tellimuse id rentniku toomiseks kasutada:

        azure account show

     Mis annab:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Kui vajate mõne muu tellimuse rentniku ID-d, kasutage järgmist käsku:

        azure account show -s {subscription-id}

1. Serdi sõrmejälje tuua ja eemaldage tarbetud märgid, kasutage.

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Mis tagastab väärtuse sõrmejälje sarnased:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Kui teil on vaja tuua kliendi id logimiseks kasutada, kasutage.

        azure ad sp show -c exampleapp

     Sisselogimiseks kasutatava väärtuse on loetletud teenuse põhilist nime guid.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Logige sisse teenuse põhilise.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

Nüüd on kinnitatud, nagu põhisumma teenuse Active Directory rakendus, mida olete loonud.

## <a name="sample-applications"></a>Lihtsad rakendused

Kuidas sisse logida teenuse peamise valimi järgmiste rakenduste kuvamine

**.NET-I**

- [Juurutamine on SSH lubatud VM malli .net-i abil](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Azure'i ressursse ja .net-i ressursikeskuse rühmade haldamine](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Java kasutamise alustamine – juurutada Azure ressursihaldur malli abil - ressursse](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Java kasutamise alustamine – haldamine ressursirühm - ressursid](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

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
  
- Üksikasjalikud juhised rakenduse integreerimine Azure ressursside haldamise kohta leiate teemast [arendaja juhend autoriseerimine Azure'i ressursihaldur API-ga](resource-manager-api-authentication.md).
- Serdid ja Azure CLI kasutamise kohta lisateabe saamiseks vaadake [serdi autentimise abil Azure'i teenus põhisumma Linuxi käsureal](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
