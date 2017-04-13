
<properties
    pageTitle="Turvaline rakendused ja ressursside Azure RemoteApp | Microsoft Azure'i"
    description="Saate teada, kuidas lukustada rakendused ja ressursside Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Turvaline rakendused ja ressursside Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp võimaldab kasutajatel juurdepääs keskselt hallatavate Windowsi rakendused, mis võimaldab teil määrata teie kasutajad saavad ja ei saa teha.  See on eriti kasulik, kui kasutaja on ühenduse mittehallatava seadmest (nt oma isikliku Macbook) ja soovite kontroll kasutajate juurdepääsu või kogemus.

Näiteks kui kasutate Active Directory kasutaja autentimist ja soovite takistada kasutajate välja rakenduse andmete kopeerimine, saate konfigureerida Remote'i töölaua rühmapoliitika Blokeeri kasutajatele andmete kopeerimist.

Teine näide on, kui soovite blokeerida mõne kindla rakenduse oma saidikogumi Interneti-ühendus. Windowsi tulemüüri reegel, blokeerib access pildi loomisel oma saidikogumi loomine

## <a name="implementation-options"></a>Rakendamise võimalusi

  Siin on rakendamise suvandid, mida saab kasutada eraldi või paralleelselt vastavalt vajadusele.

1.  Kui teie saidikogumi RemoteApp on ühendatud domeeni saate jõustada [Rühmapoliitika](https://technet.microsoft.com/library/cc725828.aspx) (välja arvatud selle jõudeoleku ja Katkesta ajalõpp poliitikate kirjeldatud [allpool](../azure-subscription-service-limits.md)).
2.  (Kui teie saidikogumi pole ühendatud domeeni või pole õige õigusi AD) rühmapoliitika alternatiivina saate konfigureerida [Kohaliku poliitikat](https://technet.microsoft.com/library/cc775702.aspx) oma malli pilti.  Pange tähele, et selle rühma poliitikat trump Kohalikud poliitikad konflikti korral.
3.  Teatud OS/rakenduse sätted ei saa konfigureerida rühmapoliitika kaudu, kuid see võib olla kaudu registrivõtme konfigureerimisel malli pilti [RegEdit tööriista](./remoteapp-hybridtrouble.md) abil.
4.  [Windowsi tulemüüri](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) abil saate reguleerida võrgule juurdepääsu ja sealt masina, kui rakendus töötab. Lihtsalt veenduge, et vältida URL-id ja määratud siin.
5.  Saate kontrollida, millised rakenduste ja failide kasutajate käitamise [AppLockeri](https://technet.microsoft.com/library/hh831440.aspx) . Näiteks savvy kasutajad aru saada, kuidas käivitada rakendusi avaldate ei, kuid mis on saadaval selle saidikogumi loomiseks kasutatud pilt – AppLockeri saate blokeerida see.

## <a name="detailed-information"></a>Üksikasjalikku teavet

- Järgmised RDS poliitikad on tõenäoliselt kõige enam kasu.
    - [Seadme ja ressursside ümbersuunamine](https://technet.microsoft.com/library/ee791794.aspx)
    - [Printeri ümbersuunamine](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profiilid](https://technet.microsoft.com/library/ee791865.aspx).
- Pange tähele, et konfigureerida ümbersuunamine RemoteApp PowerShelli kaudu mooduli (nagu näha [siin](./remoteapp-redirection.md)) tugineb kliendi seadme jõustamiseks poliitika, et kui turvalisus on peamine eesmärk soovite rakendada poliitika malli pilti kohaliku rühmapoliitika või rühmapoliitika kaudu.
- [Windows Server 2012 R2 poliitika](https://technet.microsoft.com/library/hh831791.aspx).
- [Office 2013 poliitikat](https://technet.microsoft.com/library/cc178969.aspx) (sh [Office'i tööriistariba kohandamine](https://technet.microsoft.com/library/cc179143.aspx)).
