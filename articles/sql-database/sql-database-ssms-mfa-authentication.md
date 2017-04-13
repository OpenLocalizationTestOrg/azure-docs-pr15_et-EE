<properties
   pageTitle="SSMS kasutajatugi Azure AD MFA SQL-andmebaas ja SQL-i andmebaas | Microsoft Azure'i"
   description="Mitme tegureid autentimise kasutamine SSMS SQL-andmebaas ja SQL-i andmebaas."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS tugi: Azure'i AD MFA SQL-andmebaas ja SQL-i andmebaas

Azure'i SQL-andmebaasi ja Azure SQL-i andmebaas nüüd ühenduste toetamiseks: SQL Server Management Studio (SSMS) *Active Directory ühes kohas autentimist*kasutades. Active Directory ühes kohas autentimine on interaktiivne töövoog, mis toetab *Azure Mitmikautentimise* (MFA). Azure'i MFA aitab kaitsta juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal. See pakub mitmesuguseid võimalusi lihtne kontrollimise tugev autentimine – telefonikõne, tekstsõnumi, smart kaardid PIN-koodi või mobiilirakenduses teatis – võimaldab kasutajatel valida eelistate. Mitmikautentimise kirjelduse leiate teemast [Mitmikautentimise](../multi-factor-authentication/multi-factor-authentication.md).

SSMS, toetab nüüd.

- Interaktiivne MFA Azure AD potentsiaalselt hüpikakende dialoogiboksi väljale valideerimisreeglite.
- Mitte-interaktiivne Active Directory parool ja Active Directory integreeritud autentimise viise, mida saab kasutada paljudes erinevates rakendustes (ADO.net-i, JDBC, ODBC jne). Need kaks meetodit kunagi tulemuseks hüpikakende dialoogiboksid.

Kui kasutajakonto on konfigureeritud lubama MFA interaktiivsed autentimise töövoog nõuab täiendavad kasutaja suhtluse kaudu hüpikakende dialoogiboksides, kiipkaardi kasutamine jne. Kui kasutajakonto on konfigureeritud lubama MFA, kasutaja peate valima Azure'i ühes kohas autentimise ühenduse. Kui kasutajakonto ei nõua MFA, kasutaja saate siiski kasutada muid Azure Active Directory autentimise kaks võimalust.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universaalne autentimise piirangud SQL-andmebaas ja SQL-i andmebaas

- SSMS on praegu MFA Active Directory ühes kohas autentimise kaudu kasutada ainult tööriista.
- Ainult ühe Azure Active Directory konto logida eksemplari ühes kohas autentimist kasutades SSMS jaoks. Logi sisse, kui Azure AD mõne muu kontoga, peate kasutama SSMS muus eksemplaris. (See piirang on piiratud Active Directory ühes kohas autentimise; saate Active Directory Paroolautentimine, Active Directory integreeritud autentimise või SQL serveri autentimine serveritest sisse logida).
- SSMS toetab Active Directory ühes kohas autentimise objekti Exploreri, Päringuredaktori ja päringu poe visualiseerimiseks.
- DacFx ega skeemi Designer ei toeta ühes kohas autentimist.
- Active Directory ühes kohas autentimise MSA kontod ei toetata.
- Active Directory ühes kohas autentimist ei toetata SSMS kasutajatele, kellel on imporditud praeguse Active Directory muude Azure Active kataloogid. Need kasutajad ei toetata, kuna vaja rentniku ID valideerimiseks kontode ja on pole ette näha, et.
- Puuduvad täiendavat tarkvara nõuded Active Directory ühes kohas autentimine, välja arvatud, et kasutate toetatud versiooni SSMS.

## <a name="configuration-steps"></a>Konfigureerimistoimingute

Mitmikautentimise rakendamise jaoks on vaja nelja põhitoimingud.

1. **Azure Active Directory konfigureerimine** – lisateabe saamiseks vt [integreerimine Azure Active Directory oma kohapealse identiteedid](../active-directory/active-directory-aadconnect.md), [lisada oma domeeninime Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [toetab nüüd Microsoft Azure'i Windows Server Active Directory federation](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Azure AD kataloogi haldamise](https://msdn.microsoft.com/library/azure/hh967611.aspx)ja [haldamine Windows PowerShelli kaudu Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **MFA konfigureerimine** – üksikasjalikud juhised leiate teemast [Konfigureerimise Azure'i Mitmikautentimise](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **SQL-andmebaasi konfigureerimine või SQL Azure'i AD autentimise andmebaas** – leiate üksikasjalikud juhised leiate teemast [ühenduse loomine SQL-andmebaasi või SQL-i andmete ladu, kasutades Azure Active Directory autentimise](sql-database-aad-authentication.md).

4. **Laadige alla SSMS** – kliendi arvutisse alla laadida uusima SSMS (vähemalt August 2016), alla [Laadida SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Ühenduse loomine SSMS ühes kohas autentimist kasutades

Järgmised toimingud näitab, kuidas luua ühenduse SQL-andmebaasi või SQL-i andmebaas uusima SSMS abil.

1. Universaalne autentimist, kasutades dialoogiboksis **serveriga** ühenduse loomiseks valige **Active Directory ühes kohas autentimist**.
![universaalne 1mfa ühendamine][1]

2. Nagu tavaliselt SQL-i andmebaas ja SQL-i andmebaas jaoks peate nuppu **Suvandid** ja andmebaasi määramiseks dialoogiboksi **Suvandid** . Klõpsake nuppu **Loo**.
3. Kui kuvatakse dialoogiboks **kontosse sisse logida** , sisestage konto ja parool Azure Active Directory teie identiteedi.
![2mfa – sisselogimine][2]

    > [AZURE.NOTE] Universaalne autentimiseks kontoga, mis ei nõua MFA loote sel hetkel. Kasutajate MFA, jätkake toimige järgmiselt.
 
4. Kahe MFA häälestamise dialoogiboksid võidakse kuvada. See üks kord toiming sõltub MFA administraatoriõigustega ja seetõttu võib olla valikuline. MFA lubatud domeeni see toiming on mõnikord eelmääratletud (nt domeeni nõuab kasutajatele kiipkaardi ja PIN-koodi).  
![3mfa-häälestamine][3]

5. Teine võimalik üks kord dialoogiboksi saate valida oma autentimise meetodit üksikasju. Teie administraator on konfigureeritud võimalusi.
![4mfa-kinnitamine-1][4]
 
6. Azure Active Directory saadab teile kinnitamiseks teave. Kui teile saadetakse kinnituskood, sisestage see väljale **Enter kinnituskood** ja valige **sisselogimine**.
![5mfa-kinnitamine-2][5]

Kui kinnitamine on lõpule jõudnud, loob SSMS tavaliselt eeldusel, et sobiv mandaate ja tulemüüri juurdepääsu.

##<a name="next-steps"></a>Järgmised sammud  

Teistele anda juurdepääsu oma andmebaasi: [SQL-i andmebaasi autentimise ja luba: juurdepääsu andmise](sql-database-manage-logins.md)  
Veenduge, et teised saaksid ühendust tulemüüri kaudu: [Azure'i SQL-andmebaasi server taseme tulemüüri reegli Azure'i portaalis konfigureerimine](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

