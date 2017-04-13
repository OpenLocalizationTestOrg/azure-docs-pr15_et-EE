<properties
   pageTitle="Tehniline eeltingimuste loomise virtual arvuti pildi Azure'i turuplatsil | Microsoft Azure'i"
   description="Mõista loomine ja juurutamine virtuaalse masina pilt Azure'i turuplatsi teistele ostmiseks."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
  ms.service="marketplace"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="Azure"
  ms.workload="na"
  ms.date="04/29/2016"
  ms.author="hascipio; v-divte"/>

# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Tehniline eeltingimuste Azure'i turuplatsil virtual arvuti pildi loomine
Protsessi põhjalikult enne alustamist lugege ja mõista, miks ja kuhu iga toimingu sooritatakse. Võimalikult palju, saate peaks oma ettevõtte andmete ja muude andmete ettevalmistamine, vajalikud tööriistad ja/või luua tehnilise komponendid enne pakkumise loomisprotsessi. Nendeks peaks olema selge selles artiklis läbivaatamine.  

## <a name="download-needed-tools--applications"></a>Laadi alla vajalikud tööriistad ja rakendused
Peaks olema valmis enne selle toimingu algust järgmist:

- Sõltuvalt sellest, milline opsüsteem on suunatud, installige [Azure'i PowerShelli cmdlet-käskude](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) või [Linuxi käsurea liides tööriista](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) [Azure'i](https://azure.microsoft.com/downloads/) allalaadimislehelt.
- Installige Azure'i salvestusruumi Exploreri CodePlex.
- Laadige alla ja installige Azure'i sertifitseeritud tööriista sertimine Test:
  - [http://go.microsoft.com/fwlink/?LinkID=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Peate sertimine tööriista Windowsi-põhises arvutis. Kui teil pole saadaval Windowsi-põhises arvutis, saate käivitada tööriista abil Windowsi-põhiste VM Azure.

## <a name="platforms-supported"></a>Toetatud platvormid
Saate arendada Linux või Windows Azure'i VMs. Avaldamise protsessi – näiteks luua mõne Azure'i ühilduv virtuaalse kõvaketta (VHD)--kasutamine eri tööriista ja juhised, olenevalt sellest, milline opsüsteem on loodud teatud elemente:  

- Kui kasutate Linux, viidata [virtuaalse masina pilt avaldamise juhend](marketplace-publishing-vm-image-creation.md)"Loo on Azure ühilduv VHD (Linux-põhine)" jaotises.
- Kui kasutate opsüsteemi Windows, viidata [virtuaalse masina pilt avaldamise juhend](marketplace-publishing-vm-image-creation.md)"On Azure ühilduv VHD (Windowsi-põhises) loomine" jaotis.

> [AZURE.NOTE] Teil on vaja juurdepääsu Windowsi-põhises arvutis, et:
- Käivitage tööriist sertimine valideerimine.
- Looge VHD ühiskasutusse Accessi allkirja URL-i esitamise VHD sertimine.

## <a name="develop-your-vhd"></a>Arendada oma VHD
Saate arendada Azure'i VHDs pilveteenuses või kohapealse:

- Pilvepõhine arengu tähendab, et kõik arengu juhiseid teostamise kaugühenduse teel on VHD elavad Azure.
- Kohapealse areng nõuab allalaadimine on VHD ja selle abil kohapealse taristu arendamise. Kuigi see on võimalik, me ei soovita seda. Pange tähele, et Windowsi või SQL-i arendamise kohapealse teil on oluline kohapealse litsentsi võtmed. Te ei saa kaasata või SQL Serverit installida VM loomise järel. Samuti peab teie pakkumise aluseks kinnitatud SQL Azure'i portaalis pilt. Kui otsustate arendada kohapealse, peate tegema mõned juhised erinevalt kui olid välja pilveteenuses. Olulise teabe leiate loomine [kohapealse VM pilt](marketplace-publishing-vm-image-creation-on-premise.md).

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui eeltingimused läbi vaadata ja vajalikud toimingud lõpule viidud, saate liikuda edasi luua oma virtuaalse masina pildi pakkumine [virtuaalse masina pilt avaldamise juhend](marketplace-publishing-vm-image-creation.md)üksikasjalikult kirjeldatud.

## <a name="see-also"></a>Vt ka
- [Alustamine: Azure'i turuplatsi avaldamine pakkumise kohta](marketplace-publishing-getting-started.md)
- [Töötab Windows Azure'i eelvaade portaali loomine](../virtual-machines/virtual-machines-windows-hero-tutorial.md)


[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
