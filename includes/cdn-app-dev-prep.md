## <a name="prerequisites"></a>Eeltingimused

Enne saame kirjutada CDN halduse kood, läheb vaja teha mõned ettevalmistamine meie kood Azure'i ressursihaldur suhelda.  Selle tegemiseks peate:

* Ressursirühm sisaldama loome selles õpetuses CDN profiili loomine
* Azure Active Directory esitada meie rakenduse autentimise konfigureerimine
* Õiguste rakendada ressursirühma nii, et ainult volitatud kasutajate meie Azure AD rentniku saab interaktiivselt kasutada meie CDN profiil

### <a name="creating-the-resource-group"></a>Ressursi rühma loomine

1. Logige [Azure portaali](https://portal.azure.com).

2. Klõpsake vasakus ülanurgas, ja seejärel **haldus**ja **Ressursirühm**nuppu **Uus** .
    
    ![Uue ressursirühma loomine](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Helistage oma ressursirühm *CdnConsoleTutorial*.  Valige oma tellimus ja asukoha lähedal.  Soovi korral võite klõpsata **Kinnita armatuurlaua** ruut, et kinnitada ressursirühma armatuurlaud portaalis.  See teeb leidmist.  Kui olete soovitud valikud teinud, klõpsake nuppu **Loo**.

    ![Nime andmise ressursirühma](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Pärast ressursirühma on loodud, kui ei kinnitate armatuurlauale, leiate, klõpsates nuppu **Sirvi**ja seejärel **Ressursi rühmad**.  Klõpsake selle avamiseks ressursirühma.  Kirjutage oma **Tellimuse ID**.  Läheb vaja seda hiljem.

    ![Nime andmise ressursirühma](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Azure AD teenuserakenduse loomine ja õiguste määramine

On kaks võimalust rakenduse autentimisega Azure Active Directory: üksikkasutajate või teenuse subjekt. Peamine teenus on sarnane teenusekonto Windows.  Asemel õiguse teatud kasutaja õiguste CDN profiilid suhelda, selle asemel anda õigused põhisumma teenusega.  Teenuse põhisumma on tavaliselt kasutatakse automatiseeritud, mitte-interaktiivne protsessid.  Ehkki selles õpetuses kirjutamise interaktiivne konsooli rakendus, me keskenduda teenuse põhisumma moodust.

Teenuse põhilise loomine koosneb mitmest etapist, sh loomise rakendus Azure Active Directory.  Selleks ei kavatse [järgige selles õpetuses](../articles/resource-group-create-service-principal-portal.md).

> [AZURE.IMPORTANT] Kindlasti [lingitud õpetuse](../articles/resource-group-create-service-principal-portal.md)kõik juhised.  On *väga oluline* , et te täitke täpselt nii, nagu on kirjeldatud.  Veenduge, et märkida oma **rentniku ID** **rentniku domeeni nimi** (tavaliselt on *. onmicrosoft.com* domeeni juhul, kui teie määratud kohandatud domeeni), **Kliendi ID**ja **Kliendi autentimise võti**, kui peame hiljem.  Olla väga ettevaatlik, et teie **Kliendi ID** ja **Kliendi autentimise võti**, nagu neid mandaate igaüks saab käivitada toiminguid nagu teenuse peamine. 
>   
> Kui samm [konfigureerimine mitme rentniku rakendus](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application), valige **ei**.
> 
> Kui juhisega [rakenduse rolli määramine](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role), kasutada lõime varasemas versioonis, *CdnConsoleTutorial*ressursirühma, kuid mitte **lugeja** rolli, määrata **CDN profiili kaasautori** roll.  Pärast saate määrata rakendus **CDN profiili kaasautori** roll ressursirühma, naaske selles õpetuses. 

Kui olete loonud teenust põhisumma ja määratud **CDN profiili kaasautori** roll, **kasutajate** tera ressursirühma peaks välja nägema umbes järgmine.

![Kasutajate blade](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Interaktiivne kasutaja autentimine

Kui asemel teenuse põhilise, oleks pigem interaktiivse kasutaja autentimine, protsess on väga sarnane teenuse subjekt.  Tegelikult peate kasutage sama toimingut, kuid mõned muudatused.

> [AZURE.IMPORTANT] Ainult järgige järgmisi toiminguid, kui otsustasite kasutada kasutaja autentimise asemel põhisumma teenus.

1. Rakenduse, mitte **Veebirakenduse**loomisel valida **kohalikus rakenduses**. 
    
    ![Kohalikus rakenduses](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Järgmisel lehel kuvatakse teilt **URI ümber suunata**.  URI ei saanud kontrollida, kuid pidage meeles, et sisestatud.  Peate hiljem. 

3. Ei ole vaja luua **Kliendi autentimise võti**.

4. Teenuse põhilise määramisel **CDN profiili kaasautori** roll, mitte ei kavatse määrata kasutajatele või rühmadele.  Selles näites näete, et olen pandud *CDN Demo kasutaja* **CDN profiili kaasautori** roll.  
    
    ![Üksikute kasutajate juurdepääsu](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

