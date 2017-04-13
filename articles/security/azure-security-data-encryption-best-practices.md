<properties
   pageTitle="Andmete turvalisus ja krüptimine head tavad | Microsoft Azure'i"
   description="Selles artiklis on toodud kogum, head tavad andmete turvalisuse ja krüptimise abil ehitatud Azure võimalusi."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Azure'i andmete turvalisus ja krüptimine head tavad

Andmekaitse pilveteenuses kiirklahvidele on raamatupidamine võimalike riikide andmete võib ilmneda ja milliseid juhtelemente riigi jaoks saadaval. Selleks, et Azure'i andmed saab ümber järgmised andmed olekus turvalisus ja krüptimine head tavad soovitused:

- Ülejäänud veebisaidil: See hõlmab kogu teabe salvestusruumi objektid, ümbriste ja tüüpi, mis on olemas staatiliselt füüsilise meedia, tuleb see magnetiline või optilise ketas.

- Teel sisse: Kui andmeid kantakse üle komponendid, asukohtade või programme, näiteks üle võrgu, üle teenuse siini (alates kohapealse Cloud ja vastupidi, sh hübriid ühendused, nt ExpressRoute) või sisend käigus see on arvatavasti nagu oleks liikumise.

Selles artiklis me arutada Azure'i andmete turvalisus ja krüptimine head tavad kogum. Järgmistest headest tavadest on tuletatud kogemused Azure andmete turvalisus ja krüptimise ja kogemused, näiteks ise.

Iga parimal viisil selgitame:

- Mis on parim
- Miks te soovite lubada selle parimaid tavasid
- Kui te ei luba parim, mis võib olla tingitud
- Võimalikud alternatiivid parim
- Kuidas saate teada, et lubada parim

Azure'i andmete turbe- ja krüptimise head tavad artiklit põhineb konsensuse arvates ja Azure'i platvormi võimaluste ja funktsioon määrab, olemasolevate see artikkel on kirjutatud ajal. Aja jooksul muutuda arvamust ja tehnoloogiad ja selles artiklis värskendatakse regulaarselt taguse.

Azure'i andmete turvalisus ja krüptimine head tavad selles artiklis kirjeldatud järgmised.

- Mitmikautentimise Jõusta
- Kasutage rollipõhise juurdepääsu reguleerimine (RBAC)
- Krüpti Azure'i virtuaalmasinates
- Riistvara turvalisus mudelite kasutamine
- Hallata Secure töökohtade
- SQL-i andmete krüptimine
- Töödeldavate andmete kaitsmiseks
- Jõusta faili taseme andmete krüptimine


## <a name="enforce-multi-factor-authentication"></a>Mitmikautentimise Jõusta

Esimene samm juurdepääs andmetele ja Microsoft Azure'i juhtelement on autentida. [Azure'i mitme teguriga autentimine (MFA)](../multi-factor-authentication/multi-factor-authentication.md) on meetod kontrollima kasutaja identiteet mõne muu meetodi, kui ainult kasutajanime ja parooli abil. See autentimise meetodit aitab kaitse juurdepääs andmetele ja rakenduste kasutaja nõudmisel lihtsa sisselogimise protsessi koosoleku ajal.

Võimaldades Azure'i MFA kasutajate jaoks, on teine kiht turvalisus lisamine kasutaja sisselogimist ja tehingud. Sel juhul võib tehingu juurdepääs failiserverisse või oma SharePoint Online'i asuva dokumendi. Azure'i MFA aitab ka seda, et vähendada tõenäosust rikutud mandaati pääsevad ettevõtte andmeid.

Näide: kui soovite Jõusta Azure'i MFA kasutajate jaoks konfigureerida nii, et kasutada telefonikõne või teksti sõnumi kinnitamine, kui kasutaja mandaat on rikutud, ründaja ei saa mis tahes ressursi juurde, kuna ta neil pole juurdepääsu kasutaja telefoni. Ettevõtted, mis ei lisa see eest kiht identiteedi kaitse on rünnak mandaati varastamine, mis võib tingida andmete kompromiss.

Üks alternatiiv ettevõtted, mida soovite säilitada autentimise juhtelemendi asutusesisese on [Azure mitmekordne autentimise Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), nimetatakse ka kohapealse MFA kasutada. Selle meetodi abil siiski saa Jõusta mitmikautentimise, säilitades MFA serveri asutusesisene.

Azure'i MFA kohta lisateabe saamiseks lugege artiklit [Azure'i Mitmikautentimise pilveteenuses töötamise alustamine](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Kasutage rollipõhise juurdepääsu reguleerimine (RBAC)
[Teadma](https://en.wikipedia.org/wiki/Need_to_know) ja [vähimate](https://en.wikipedia.org/wiki/Principle_of_least_privilege) turvalisus põhimõtteid juurdepääsu piirata. See on väga oluline ettevõtted, mida soovite rakendada turbepoliitikate andmepääsu. Azure'i Rollipõhine juurdepääsu juhtimine (RBAC) saab kasutajate, rühmade ja rakendused teatud ulatusega õiguste määramiseks. Rollimäärang ulatus võib olla tellimuse, ressursirühma või ressurss.

Saate kasutada [Sisseehitatud RBAC-rollid](../active-directory/role-based-access-built-in-roles.md) Azure kasutajatele õiguste määramiseks. Kaaluge *Salvestusruumi konto kaasautor* cloud tehtemärgid, mida on vaja salvestusruumi kontod ja *Klassikaline salvestusruumi konto kaasautori* roll haldamiseks klassikaline salvestusruumi kontod haldamine. Pilveteenuse tehtemärgid, mida tuleb VMs ja salvestusruumi konto haldamine, kaaluge lisamine *Virtuaalse masina kaasautori* roll.

Ettevõtted, mis kehtestavad andmete juurdepääsu reguleerimine, nt RBAC võimaluste kasutamine võib anda rohkem õigusi, kui on vaja oma kasutajate jaoks. See võib põhjustada andmete kompromiss on mõned kasutajad, kellel on juurdepääs andmeid, mida nad ei peaks olema kõigepealt.

Lisateavet Azure RBAC leiate artiklist [Azure Role-Based juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Krüpti Azure'i Virtuaalmasinates
Paljud ettevõtted, [krüptimist ja ülejäänud andmed](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) on kohustuslikud samm andmete privaatsust, nõuetele vastavus ja andmete suveräänsuse. Azure'i ketta krüptimise võimaldab IT-administraatorid Windows ja Linux IaaS virtuaalse masina (VM) ketast krüptida. Azure'i ketta krüptimise mõjutab valdkonna standard BitLockeri funktsioon Windowsi ja Linux esitada krüptimine OS ja andmete ketast DM-Crypt funktsioon.

Saate kasutada Azure ketta krüptimise kaitsta ning kaitsta oma andmeid ja vastavad teie ettevõtte Turve ja nõuetele. Ettevõtted peaksite kaaluma krüptimise abil seotud volitamata andmepääsu riske leevendada. Samuti on soovitatav krüptida draivid enne nende tundliku loomuga andmete kirjutamise.

Veenduge, et teie VM andmemahtudega ja buutimine helitugevuse krüptimiseks ülejäänud Azure storage konto andmete kaitsmiseks. [Azure'i klahvi Vault](../key-vault/key-vault-whatis.md)tehtavate kaitsta krüptimise võtmed ja saladused.

Oma kohapealse Windows Server, kaaluge järgmisi krüptimise head tavad:

- Kasutage andmete krüptimiseks [BitLockeri abil](https://technet.microsoft.com/library/dn306081.aspx)
- Talletage taastamise teave AD DS-is.
- Kui mis tahes muret, et BitLockeri klahvid on rikutud, soovitame, et saate vormingu eemaldada kõik eksemplarid BitLockeri metaandmete ketas ketas või olete dekrüptida ja krüptimine kogu ketas uuesti.

Ettevõtted, mis kehtestavad andmete krüptimine tõenäoliselt rohkem andmete terviklusega seotud probleemide nagu varastamine andmete pahatahtlik või rogue kasutajad kokku puutuda ja rämpsposti kontod volitamata juurdepääsu andmete Eemalda vorming. Lisaks neid riske ettevõtted, kes on valdkonna eeskirjadele, peab osutuda nende hoolas ja kasutate õiget turbemeetmed andmete turvalisuse täiustamiseks.

Saate selle kohta leiate lisateavet Azure ketta krüptimise artiklist [Azure'i ketta krüptimise for Windows ja Linux IaaS VMs](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Kasutage riistvara turvalisus moodulid

Valdkonna krüptimise lahenduste salajane klahvide abil krüptida andmeid. Seetõttu on oluline, et need turvaline salvestatakse. Võtme haldamise muutub osaks andmekaitse, kuna see on kasutada salajane klahvid, mida kasutatakse andmete krüptimiseks talletamiseks.

Azure'i ketta krüptimine kasutab [Azure klahvi Vault](https://azure.microsoft.com/services/key-vault/) aitab teil määrata, ja ketta krüptimise võtmed ja saladusi tellimuse võtme vault, tagades, et virtuaalse masina ketta kõik andmed on krüptitud korral Azure salvestusruumi haldamine. Kasutage Azure'i klahvi Vault auditeeritavad võtmed ja Poliitikakasutuse.

On palju riske, mis on seotud ei ole vastav turbemeetmed salajane klahvid, mida kasutati andmete krüptimiseks kaitsmiseks. Kui ründajad on juurdepääs salajane klahvid, ta saab andmed dekrüptida ning potentsiaalselt juurdepääs konfidentsiaalset teavet.

Te saate lisateavet üldised soovitused serdihaldus Azure artikli lugemine [Serdihaldus Azure: käsud ja keelud](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Azure'i klahvi Vault kohta lisateabe saamiseks lugege [Azure'i klahvi Vault alustamine](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Hallata Secure töökohtade

Kuna selle eest enamik suunata lõppkasutaja, lõpp-punkti muutub üks esmane rünnak. Kui ründaja kompromisse lõpp-punkti, ta saate kasutada kasutaja mandaat juurde pääseda ettevõtte andmeid. Enamik lõpp-punkti eest saavad ära asjaolu, et kasutajad on oma kohaliku töökohtade administraatoritele.

Turvaline haldus töökoha abil saate neid riske vähendada. Soovitame kasutada [Õigustega juurdepääsu töökohtade (PAW)](https://technet.microsoft.com/library/mt634654.aspx) vähendada rünnak pind töökohtade sisse. Nende töökohtade turvaline haldus aitab teil leevendada mõned neist eest aidata tagada, et teie andmed on turvalisem. Veenduge, et kasutada PAW kõvaks ja oma töökoha lukustada. See on oluline samm turvatud kinnitused tundliku kontode, ülesannete ja andmekaitse esitada.

Lõpp-punkti kaitse puudumine võib teie andmed panema ohtu, veenduge, et turvalisus poliitikaid rakendada kõigis seadmetes kasutamine andmete andmete asukoht (pilve või kohapealse) sõltumata kasutatud.

Lugege lisateavet õigustega juurdepääs töökoha artiklist [Turvaliseks õigustega juurdepääs](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>SQL-i andmete krüptimine

[Azure'i SQL-andmebaasi läbipaistvaid andmete krüptimine](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) kaitseb pahatahtlik tegevus ohtu nõudmata rakenduse muudatusi reaalajas krüptimine ja dekrüptimine andmebaasi, seotud varukoopiate ja tehingulogi failide veebisaidil ülejäänud järgige järgmisi juhiseid.  TDE krüptib kogu andmebaasi salvestusruumi sümmeetriline võti nimega andmebaasi krüptovõtme abil.

Isegi siis, kui kogu salvestusruumi on krüptitud, on väga oluline ka ise andmebaasi krüptimine. See on kaitse sügavus lähenemisviisi andmekaitse rakendamist. Kui kasutate [Azure SQL-andmebaas](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) ja soovite kaitsta tundlikku andmeid, nt krediitkaardi või isikukoodid, saate krüptida andmebaasid FIPS 140-2 valideeritud 256 bit AES krüptimist, mis vastab paljudele tööstusstandarditele (nt HIPAA, PCI).

See on oluline mõista, et kui andmebaas on krüptitud TDE pole krüptitud failide [puhvri pool pikendamise](https://msdn.microsoft.com/library/dn133176.aspx) (BPE). Kasutage faili krüptimise taseme Süsteemiriistad nagu BitLockeri või [Failisüsteemi](https://technet.microsoft.com/library/cc700811.aspx) (EFS) BPE seotud faile.

Alates autoriseeritud kasutaja nagu turvalisus administraatori või oma andmebaasi administraatorilt pääsevad andmetele isegi siis, kui andmebaas on krüptitud TDE, tuleks ka järgite järgmisi soovitusi:

- SQL-i autentimise andmebaasi tasemel
- Azure'i AD autentimist kasutades RBAC rollid
- Kasutajate ja rakenduste tuleks kasutada eraldi kontod autentida. Nii saate piirata kasutajate ja õiguste ja riske pahatahtlik tegevus
- Kirjehalduse andmebaasi taseme turvalisus, kasutades fikseeritud andmebaasi rollid (nt db_datareader või db_datawriter), või saate luua kohandatud rollid rakenduse valitud andmebaasiobjektide konkreetsete õiguste andmine

Ettevõtted, mis ei kasuta andmebaasi krüptimine taseme võivad mõjutada rohkem jaoks eest, mis asub SQL andmebaase andmeid kahjustada.

Saate selle kohta leiate lisateavet SQL-i TDE krüptimise artiklist [Läbipaistvaid andmete krüptimine koos Azure'i SQL-andmebaasi](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Töödeldavate andmete kaitsmiseks

Töödeldavate andmete kaitsmine peaks olema oluline osa strateegia kavandamine andmed. Kuna andmete liigub edasi ja tagasi palju asukohtadest, üldine soovitus on alati kasutada SSL/TLS-protokolli vahetada andmeid mitmest eri kohti. Mõnel juhul võite soovida oma kohapealse ja pilveteenuse vahel kogu suhtlus kanali eristamiseks taristu virtuaalse privaatvõrgu (VPN) abil.

Andmete teisaldamine kohapealsesse taristu ja Azure, kaaluge vastav kaitse, nt HTTPS või VPN.

Ettevõtted, mis tuleb tagada juurdepääs mitmele töökohtade asub kohapealse Azure, kasutage [Azure - saidilt VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Ettevõtted, mis on vaja turvalist ühe töökoha asub kohapealse juurdepääsu Azure, kasutada [punkti saidi VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Komplekti saab teisaldada sihtotstarbeline kiire WAN üle suuremat andmete linkimine nagu [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Kui otsustate kasutada ExpressRoute, saate ka krüptida rakenduse tasemel andmete [SSL/TLS](https://support.microsoft.com/kb/257591) -i või muud Protokollid abil lisatud kaitse.

Kui suhtlete Azure Storage Azure portaali kaudu, tehingud tekkida HTTPS-i kaudu. [Salvestusruumi REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) https saate kasutada ka suhelda [Azure Storage](https://azure.microsoft.com/services/storage/) ja [Azure SQL-andmebaas](https://azure.microsoft.com/services/sql-database/).

Ettevõtted, mis ei töödeldavate andmete kaitsmiseks on [man-in-Lähis eest](https://technet.microsoft.com/library/gg195821.aspx), [pealtkuulamise](https://technet.microsoft.com/library/gg195641.aspx) ja seansi ärandamine. Nende eest, võib olla esimene samm juurdepääsu konfidentsiaalseid andmeid.

Leiate Azure'i VPN suvandite kohta lisateavet artiklist [kavandamisel ja kujunduse jaoks VPN-lüüsi](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Jõusta faili taseme andmete krüptimine

Veel üks kaitse, mida saate suurendada turvalisus teie andmete jaoks on krüptimine faili, olenemata faili asukoht.

[Azure RMS -i](https://technet.microsoft.com/library/jj585026.aspx) kasutab krüptimist, identiteedi ja luba turvalisemaks faile ja e-posti. Mitmes seadmes toimib Azure RMS-telefonide, tahvelarvutite ja PC kaitse nii oma ettevõttes kui ka väljaspool teie asutust kaudu. See funktsioon on võimalik, kuna Azure RMS-i lisab kaitsetase, mis jääb andmetega, isegi siis, kui see jätab ettevõtte piirid.

Azure RMS-i kasutamisel failide kaitsmiseks kasutate industry-standard krüptograafia täielik tugi [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Kui Azure RMS-i saate kasutada andmete kaitsmise, peate assurance, mis jääb kaitse faili, isegi juhul, kui see kopeeritakse salvestusruumi, mis pole, nt pilvepõhine salvestusteenus kontrolli all. Sama ilmneb e-post kaudu ühiskasutusse antud faili fail on kaitstud juhistega meilisõnumi manusena kaitstud manust avada.

Azure RMS-i vastuvõtmiseks kavandamisel soovitame järgmist:

- Installige [RMS-i ühiskasutus rakendus](https://technet.microsoft.com/library/dn339006.aspx). See rakendus ühendab Office'i rakendused installida Office'i lisandmoodul, et kasutajad saavad hõlpsalt kaitsta faile otse.
- Rakenduste ja teenuste tugi Azure RMS-i konfigureerimine
- Luua [Kohandatud mallid](https://technet.microsoft.com/library/dn642472.aspx) , mis vastavalt oma ettevõtte nõuetele. Näide: ülemise salajane andmed, mis rakendatakse kogu salajase malli seotud meilisõnumid.

Ettevõtted, mis on [andmete liigitamine](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ja faili nõrk võib mõjutada rohkem andmeleket. Ilma õige failikaitse, ettevõtted ei saa saada äri teadmisi, kuritarvitamiseks jälgimine ja failide pahatahtlik juurdepääsu vältimiseks.

Lisateavet Azure RMS-i leiate artiklist, mis on [Azure Rights Management töötamise alustamine](https://technet.microsoft.com/library/jj585016.aspx).
