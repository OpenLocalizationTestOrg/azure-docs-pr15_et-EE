<properties
   pageTitle="SQL Azure'i Azure RemoteAppi | Microsoft Azure'i"
   description="Siit saate teada, kuidas kasutada SQL Azure'i Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
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

# <a name="sql-azure-with-azure-remoteapp"></a>SQL Azure'i Azure RemoteAppi

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Sageli siis, kui kliendid valida majutada taotluste Windows Azure'i RemoteAppi pilveteenuses soovivad ka oma andmeid nagu SQL-i serverite migreerimine pilve kogu cloud juurutamine. See võimaldab kogu pilve majutatud lahenduse, millele pääseb igal ajal mis tahes seadme abil kõikjal Azure RemoteApp. Allpool leiate linke ja juhiseid, et aidata teil seda protsessi koos viited.  

## <a name="migrate-your-sql-data"></a>SQL-i andmete migreerimine

Alustage [migreerimine SQL serveri andmebaasi Azure SQL-andmebaasiga](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Azure RemoteApp konfigureerimine
Rakenduse Windows Azure RemoteApp majutada. Allpool on väga kõrge samm-sammult:

1.     Saate luua [malli Azure RemoteApp VM](remoteapp-imageoptions.md). 
2.     Vajalik rakendus installida VM.
3.     Rakenduse konfigureerida nii, et see loob ühenduse SQL DB ja veenduge, et see toimib.
4.     Sysprep ja seiskamise VM. See jäädvustada pildina Azure'i jaoks. **Märkus:** Peate veenduge, et rakendus on võimalik säilitada DB ühenduvuse teabe sysprep protsessi kaudu. Kui rakendus ei saa DB ühenduse teavet säilitada, võite kaasata tarnija taotluse kontrollida, kuidas me ei saa määrata ühendusstring.
5.     Importida kohandatud pildi valimine proper geograafiline asukoht, mis asub teie SQL Azure'i juurutamise Azure RemoteApp teeki. 
6.     Juurutada RemoteApp saidikogumi sama andmekeskuse juurutamise SQL Azure'i ülaltoodud malli abil ja rakenduse avaldamine. Juurutamise juurutamine Azure RemoteApp sama nimega oma SQL Azure'i andmekeskuse aitab tagada kiireim ühenduse kiirust ja vähendada latentsus. 

## <a name="app-and-sql-configuration-considerations"></a>Rakenduse ja SQL-i konfigureerimise kaalutlused
On mõned punktid, millega tuleks arvestada RemoteAppi Azure SQL-i kasutamisel.

Saate teada, [Kuidas konfigureerida mõnda SQL Azure'i andmebaasi tulemüüri](../sql-database/sql-database-firewall-configure.md). Väljavõte artiklist olekud: "esialgu kõik juurdepääsu oma Azure SQL-andmebaasi server on blokeeritud tulemüür. Hakata oma Azure SQL-andmebaasi server, peate klassikaline portaalis ja määrata ühe või mitme serveritaseme tulemüüri reeglid, mis võimaldavad juurdepääsu oma Azure SQL-andmebaasi server. Funktsiooni tulemüüri reeglite abil määrata, milline IP-aadresside vahemikud Interneti kaudu on lubatud ja seda, kas Azure rakendusi saate Azure SQL-i andmebaasiserveriga ühenduse loomiseks."

Lisaks, kui arvuti püüab Interneti kaudu oma andmebaasi serveriga ühendust luua, tulemüüri kontrollib taotluse vastu serveritaseme täiskomplekti pärit IP-aadress (vajadusel) andmebaasi taseme tulemüüri reeglid. "Kui taotluse IP-aadress on üks serveritaseme tulemüüri reeglid määratud vahemikud, ühenduse antakse oma Azure SQL-andmebaasi server." Seega saate teha IP-vahemikke kasutus ja mitte ainult üksikute allika IP-aadressid.

Järgige samm-sammult [kohta: tulemüüri sätete konfigureerimine portaalis Azure SQL-andmebaasi](../sql-database/sql-database-configure-firewall-settings.md) IP vahemiku. SQL-i tulemüüri reeglid konfigureerimisel sisestage IP alamvõrgu määratud vahemiku Azure RemoteApp saidikogumi jaoks. See peaks võimaldama ARA serveritega ühenduse SQL DB isegi juhul, kui need on dünaamiliselt-määratakse IP-aadressid.

## <a name="troubleshooting"></a>Tõrkeotsing
Kui kliendi rakendus kasutamise kogemus majutatud Azure RemoteApp, mis loob ühenduse SQL andmebaasi kui majutatud Azure'i või kohapealse on aeglane põhjusi võib olla mõni miks.  

- Võrgu kaudu oma seadme Azure latentsusaeg suur. Parima jõudluse saate parim ja kiireim võrguühendus minek Teie seadmetes latentsus Azure andmekeskuse testimiseks kasutada [azurespeed.com](http://azurespeed.com/) üldine tööriist.  
- Majutatud Azure RemoteApp kliendi rakendus on stressis. Ruudu arvelduse lepingule, nt Premium arveldamine parandab jõudlust. Mõne muu lahenduseks on rakenduse tarbib ressursside jälgimine: aktiivse seansi ajal teha klahvikombinatsioon ctrl-alt-end, mis käivitab SAS Kuva, valige Tegumihaldur ja jälgida oma rakenduse ressursi kasutamine.
- SQL serveri stressis on või pole optimeeritud. Järgige SQL juhised tõrkeotsing. 

