<properties
   pageTitle="Käivitage mõni rakendus Windows Azure'i RemoteAppi mis tahes seadmes | Microsoft Azure'i"
   description="Saate teada, kuidas mõni rakendus Windows Azure RemoteApp abil kasutajatega ühiskasutatava."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Käivitage mis tahes rakendus Windows Azure'i RemoteAppi mis tahes seadmes

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Käivitada suvalist Windowsi rakendus mis tahes seadmes kohe, oluliselt - lihtsalt Azure RemoteApp abil. Kas see on kohandatud rakenduse kirjutada 10 aastat tagasi või Office'i rakenduse, kasutajate enam teatud operatsioonisüsteemi (nt Windows XP) nende paari rakenduste seotakse.

Azure'i RemoteAppi, kasutajate saate kasutada oma Android- või Apple'i seadmete ja sama kogemust, kas kasutate Windows (või Windowsi telefonides). See on võimalik majutada oma Windowsi rakenduses kogumi virtuaalmasinates Windows Azure – teie kasutajad pääsevad kõikjal nad on Interneti-ühendus. 

Lugege edasi näide täpselt, kuidas seda teha.

Selles artiklis ei kavatse jagada juurdepääs kõigile kasutajatele. Siiski saate kasutada mis tahes rakenduse. Kui installite rakenduse Windows Server 2012 R2 arvutis, saate selle alltoodud juhiseid abil ühiskasutusse anda. Saate vaadata [rakenduse nõuded](remoteapp-appreqs.md) veenduge, et teie rakendus ei tööta.

Pange tähele, et kuna on Accessi andmebaas ja soovime andmebaasi on kasulik, teeme paar lisatoimingut lasta kasutajatel juurdepääs Accessi andmete ühiskasutus. Kui teie rakendus pole andmebaasi või peate kasutajatele juurdepääsu failikettale, võite vahele jätta neid juhiseid õppeteema

> [AZURE.NOTE] <a name="note"></a>Peate selle õpetuse lõpuleviimiseks Azure'i konto.
> - Saate [avada Azure'i konto tasuta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): saate krediiti abil saate proovida makstud Azure'i teenuste ja isegi juhul, kui nad kasutada kuni hoiate konto ja kasutage tasuta Azure teenused, nt veebisaidid. Krediitkaardi kunagi tuleb tasuda, kui te just teiega sätete muutmine ja paluge võetakse.
> - Saate [aktiveerida MSDN-i abonendi eelised](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): teie MSDN-i tellimuse annab teile krediiti iga kuu makstud Azure'i teenuste kasutatavad.


## <a name="create-a-collection-in-remoteapp"></a>RemoteApp kogumi loomine

Alustuseks kogumi loomine. Selle saidikogumi toimib ümbris oma rakendused ja kasutajad. Iga saidikogumi põhineb pilt – saate luua oma või kasutada ühte esitatud tellimus. Selles õpetuses mõeldud kirjutit Office 2013 prooviversiooni pilt – see sisaldab rakendust, millega me soovite ühiskasutusse anda.

1. Azure'i portaalis, liikuge kerides allapoole vasakul navigeerimisribal puu seni, kuni näete RemoteApp. Avage lehe.
2. Klõpsake nuppu **Loo RemoteApp kogum**.
3. Klõpsake **kiire loomine** ja sisestage oma saidikogumi nimi.
4. Valige piirkond, mida soovite kasutada oma saidikogumi loomiseks. Parima kasutuskogemuse saamiseks valige piirkond, mis on kõige lähemal geograafiliselt asukoht, kuhu kasutajad pääsevad rakendus. Näiteks selles õpetuses kasutajate asub Redmond, Washington. Lähima Azure'i piirkond on **Lääne US**.
5. Valige arvelduse leping, mida soovite kasutada. Tavaline arvelduse leping paneb 16 kasutajate suur Azure VM, ajal standard arvelduse lepingul on 10 kasutajat, suure Azure'i VM. Üldine näide põhi-plaani töötab tore andmete kirje tüüp töövoo jaoks. Tootlikkuse rakenduse Office, nagu soovite siis.
6. Lõpetuseks, valige pilt Office 2013 Professional. Sellel pildil sisaldab Office 2013 rakendusi. Lihtsalt meeldetuletus – see pilt on ainult hea prooviversiooni saidikogumite ja POCs. Saate "ei saa kasutada sellel pildil tootmise kogumi.
7. Nüüd klõpsake **loomine RemoteApp kogum**.

![RemoteApp cloud saidikogumi loomine](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Käivitub teie saidikogumi loomine, kuid võib kuluda kuni tund.

Nüüd olete valmis oma kasutajaid lisada.

## <a name="share-the-app-with-users"></a>Rakenduse ühiskasutus kasutajatega

Kui teie saidikogumi on loodud, on aeg avaldada Accessi kasutajatele ja kasutajatele, kellel peaks olema juurdepääs sellele lisada.

Kui te viibite eemale Azure RemoteApp sõlm ajal kogumise loodi, alustage teed tagasi see Azure avalehel.

2. Klõpsake saidikogumi, mis on loodud varasemates täiendavad suvanditele juurdepääsuks ja konfigureerida selle saidikogumi.
![Uue RemoteApp cloud kogum](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. **Avaldamise** menüü **Avalda** ekraani allservas nuppu ja klõpsake **menüü avalda alustada programmid**.
![Avaldamine RemoteApp programm](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Valige rakendused, mida soovite loendis avaldada. Meie eesmärgil valisime juurdepääsu. Klõpsake nuppu **valmis**. Oodake, kuni rakendused avaldamise lõpuleviimiseks.
![Avaldamise juurdepääsu RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Kui rakendus on lõpule jõudnud avaldamine, pea üle vahekaardi **Kasutajate juurdepääsu** sisule juurde oma rakenduste kasutajate lisamiseks. Sisestage kasutajate kasutajanimed (e-posti aadress) ja klõpsake siis nuppu **Salvesta**.

![Kasutajate lisamine RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Nüüd on aeg oma kasutajate teavitamine need uue rakendused ja kuidas neile juurde pääseda. Selleks saata kasutajate e-posti, mis osutab kaugtöölaua kliendi alla URL.
![Kliendi jaoks RemoteApp URL-i allalaadimine](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Accessi juurdepääsu konfigureerimine

Mõni on vaja täiendavat konfiguratsiooni pärast nende juurutamist RemoteAppi kaudu. Eelkõige juurdepääsu, me ei kavatse failikettale Azure'i, mis tahes kasutaja pääseb juurde luua. (Kui te ei soovi seda teha, saate luua [hübriid saidikogumi](remoteapp-create-hybrid-deployment.md) [asemel meie pilveteenuse saidikogumi], mis võimaldab kasutajatel faile ja teavet teie kohalikus võrgus.) Seejärel läheb vaja öelda meie kasutajad oma arvuti kohalikule kettale vastendamiseks Azure failisüsteemis.

Esimene osa administraatorina saate teha. Seejärel meil on mõned juhised kasutajate jaoks.

1. Käivitage, avaldades selle käsurea liides (cmd.exe). **Avaldamise** menüü valige **cmd**ja klõpsake **Avalda > Avalda programmi teed kasutades**.
2. Sisestage nimi ja rakenduse tee. Meie eesmärgil kasutada "File Explorer" nimega "% SYSTEMDRIVE%\windows\explorer.exe" nime ja teed.
![Faili cmd.exe avaldada.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Nüüd tuleb Azure [storage konto](../storage/storage-create-storage-account.md)loomiseks. Oleme nimega meie "accessstorage", seega valige teile tähenduslik nimi. (Et laenata valesti Highlander, ei saa olla ainult üks "accessstorage.") ![Meie Azure storage konto](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Nüüd minge tagasi armatuurlauale nii, et saate tee salvestusruumi (lõpp-punkti asukoht). Kuvatakse kasutamine natuke, seega veenduge, et kopeerite kuhugi.
![Salvestusruumi konto tee](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Nüüd kui salvestusruumi konto on loodud, peate esmane juurdepääsu võti. Klõpsake nuppu **Halda kiirklahvide**ning seejärel kopeerige esmane juurdepääsu võti.
6. Nüüd seadmine kontekstis salvestusruumi konto ja luua uus fail ühiskasutus juurdepääsu. Windows PowerShelli kõrgemal aknas käivitage järgmised cmdlet-käsud:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Nii, et need on meie ühiskasutus võtame cmdlet-käsud:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Nüüd on kasutaja sisselülitamine. Esmalt laske kasutajate [RemoteAppi kliendi](remoteapp-clients.md)installida. Järgmiseks peate kasutajad selle Azure failide ühiskasutuses loodud draivi oma konto ja lisada sinna oma failidele juurdepääsemine. See on, kuidas nad teevad:

1. Access RemoteApp kliendi avaldatud rakendused. Käivitage programm cmd.exe.
2. Käivitage järgmine käsk draivi arvutist faili ühiskasutusse.

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Kui määrate parameetri Jah **/ püsiv** , Vastendatud draivi kestab kogu seansid.
1. Nüüd, käivitage rakendus File Exploreri kaudu RemoteApp. Kopeerige Accessi failid soovite ühiskasutusega faili ühiskasutusse andmine rakenduse kasutamine.
![Kasutusele mõne Azure ühiskasutus failidele juurdepääsemine](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Lõpetuseks, avage Access ja seejärel avage andmebaas, mida saate just ühiskasutuses. Peaksite nägema pilvest Accessi andmete.
![Reaal Accessi andmebaasi pilvest töötab](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Nüüd saate kasutada Accessi mis tahes seadme - lihtsalt veenduge, et installite RemoteApp kliendi.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud loomise kogumi, proovige [saidikogumi, mis kasutab teenusekomplekti Office 365](remoteapp-tutorial-o365anywhere.md). Samuti võite luua [hübriid saidikogumi ](remoteapp-create-hybrid-deployment.md), millele pääsete juurde teie kohalikus võrgus.

<!--Image references-->
 
