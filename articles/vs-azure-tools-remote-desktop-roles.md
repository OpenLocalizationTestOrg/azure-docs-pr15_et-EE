<properties 
   pageTitle="Kaugtöölaua kasutamine Azure rollid | Microsoft Azure'i"
   description="Kaugtöölaua kasutamine Azure rollid"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-remote-desktop-with-azure-roles"></a>Kaugtöölaua kasutamine Azure rollid

Azure'i SDK ja Kaugtöölauateenuseid abil pääsete Azure rollid ja mis on majutatud Azure'i virtuaalmasinates. Visual Studio, saate konfigureerida Kaugtöölauateenuseid kaudu on Azure projekt. Kaugtöölauateenuseid lubamiseks peate looma töötamine projekti, mis sisaldab ühe või mitme rollid ja seejärel Azure'i avaldamine.

>[AZURE.IMPORTANT] Peaks juurdepääsu tõrkeotsing või arendamise ainult Azure rolli. Iga virtuaalse masina eesmärk käivitamiseks kindla rolliga Azure rakenduse, mitte muude klientrakendustes käivitamiseks. Kui soovite kasutada Azure virtuaalse masina, mida saate kasutada mis tahes eesmärgil majutada, lugege teemat juurdepääs Azure'i Virtuaalmasinates Server Explorer.

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Lubada ja kasutada kaugtöölaua rolli Azure

1. Solution Exploreris oma projekti kiirmenüü avamine ja klõpsake nuppu **Avalda**.

    Kuvatakse **Avaldada Azure taotluse** viisard.

    ![Käsk pilveteenuses projekti avaldamine](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)

1. Valige viisardi **Microsoft Azure'i avaldamine sätete** lehe allosas **Lubamine kaugtöölaua** jaoks ruut Kõik rollid. 

    Kuvatakse dialoogiboks **Remote Desktop konfiguratsiooni** .

1. **Remote Desktop konfiguratsiooni** dialoogiboksi allservas nuppu **Veel suvandeid** . 
 
    Ripploendiboksi, mis võimaldab teil luua või valida sert, et kaudu kaugtöölaua ühenduse loomisel saate krüptida identimisteabe teave kuvatakse.

1. Valige rippmenüüst, ** &lt;Loo >**, või valige loendist mõne olemasoleva. 

    Kui valite lüüsiarvutis olemasolev sert, jätke.

    >[AZURE.NOTE] Kaugtöölaua ühendus soovitud serdid on Azure toimingute kasutatav serdid. Kaugpöördusteenuse sert olema privaatvõti.

    Kuvatakse dialoogiboks **Serdi loomine** .

    1. Sisestage sõbralik nimi, uut serti ja seejärel klõpsake nuppu **OK** . Ripploendiboksi kuvatakse uut serti.

    1. Sisestage dialoogiboksis **Remote Desktop konfiguratsiooni** kasutajanime ja parooli.
    
        Ei saa kasutada konto. Uue konto kasutajanimi administraator ei määrata.

        >[AZURE.NOTE] Kui parool ei vasta keerukuse nõuetele, kuvatakse punast ikooni kõrval tekstiväljale parool. Parooli peab sisaldama suurtähti, väiketähti, ja numbreid ja sümboleid.

    1. Valige konto aegub ja pärast millist Kaugtöölaua ühendused on blokeeritud.

    1. Pärast andsite kogu vajaliku teabe, klõpsake nuppu **OK** .
    
        Mitu sätet, mis võimaldavad Remote Access Services lisatakse .cscfg ja .csdef failid.

1. **Microsoft Azure'i avaldamine sätted** viisardis nuppu **OK** kui olete valmis oma pilveteenuses avaldada.

    Kui te pole avaldada, klõpsake nuppu **Loobu** . Otsingukonfiguratsiooni sätetest salvestatakse ja hiljem saate avaldada oma pilveteenuses.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Kaugtöölaua rolli Azure'i ühendamine

Pärast avaldamist oma pilveteenuses Azure, saate serveri Exploreri majutatakse Azure'i virtuaalmasinates selle sisse logida. 

1. Laiendage **Azure'i** Server Explorer, ja seejärel laiendage pilveteenus ja üks selle eksemplaride loendi kuvamiseks.

1. Mõne eksemplari sõlm kiirmenüü avamine ja valige **Ühenduse abil Kaugtöölaud**.

    ![Ühenduse kaudu kaugtöölaua](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)

1. Sisestage kasutajanimi ja parool, mida olete varem loonud. Teil on nüüd loginud oma Kaugseanss.


