<properties 
   pageTitle="Tõrkeotsing juurutatud StorSimple seade | Microsoft Azure'i"
   description="Kirjeldab, kuidas diagnoosimine ja vigade, mis ilmnevad StorSimple seadmes, mis on praegu juurutatud ja töökorras."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />

# <a name="troubleshoot-an-operational-storsimple-device"></a>Tõrkeotsing funktsionaalseid StorSimple seadmes

## <a name="overview"></a>Ülevaade

Sellest artiklist leiate abi tõrkeotsingu juhised konfiguratsiooni seotud probleemide lahendamine võib esineda pärast seadme StorSimple on juurutatud ja töökorras. Selle kirjeldatakse levinumaid probleeme, võimalikud põhjused ja soovituslikku toimingut abil saate lahendada probleeme, mis võivad ilmneda Microsoft Azure'i StorSimple käivitamisel. See teave kehtib nii StorSimple kohapealse seadme ja StorSimple virtuaalse seade.

Selles artiklis leiate tõrkekoodid, mis võivad ilmneda Microsoft Azure'i StorSimple töötamise ajal loendi lõpus juhiseid ning te saate teha tõrgete lahendamine. 

## <a name="setup-wizard-process-for-operational-devices"></a>Häälestamise viisardi protsessi funktsionaalseid seadmete jaoks

Häälestusviisardi kasutamine ([Invoke-HcsSetupWizard][1]) Kontrollige seadme konfiguratsiooni ja võtmine liikmete vajaduse korral.

Häälestusviisardi käivitamisel varem seadistatud ja töökorras seadmes voogu protsess erineb. Saate muuta ainult järgmised kirjed.

- IP-aadress, alamvõrgu mask ja lüüsi
- Esmane DNS-i server
- Esmane NTP server
- Valikuline web puhverserveri konfigureerimine

Häälestusviisardi pole parooli kogumine ja seadme registreerimine seotud toiminguid.

## <a name="errors-that-occur-during-subsequent-runs-of-the-setup-wizard"></a>Tõrked, mis ilmnevad hiljem jookseb häälestusviisardi

Järgmises tabelis kirjeldatakse tõrked võivad ilmneda käivitamisel häälestusviisardi funktsionaalseid seadmes, tõrgete võimalikke põhjusi ja Soovitatavad toimingud lahendamiseks ette võtta. 

| Ei. | Kuvatakse tõrketeade või tingimus | Võimalikud põhjused | Soovitatav toiming |
|:--- |:-------------------------- |:--------------- |:------------------ |
|  1  | Tõrge 350032: Kasutatav seade on juba inaktiveeritud. | Kui käivitate häälestusviisardi seade, mis on välja lülitatud, siis kuvatakse see tõrge. | [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) järgmised toimingud. Desaktiveeritud seade ei saa sellele teenuses. Factory reset võidakse nõuda enne seadme saab uuesti aktiveerida. |
|  2  | Autonoomsest HcsSetupWizard: ERROR_INVALID_FUNCTION (erandiks HRESULT: 0x80070001) | DNS-i serveri värskendamine nurjub. DNS-i sätted on Globaalsätete ning need rakendatakse kogu lubatud võrgu liidesed. | Luba kasutajaliides ja DNS-i sätete uuesti rakendada. See võib võrgu jaoks muu lubatud liideste katkestada, kuna need sätted on globaalne. |
|  3  | Seadme näib portaalis StorSimple halduri võrgus, kuid minimaalne häälestamise lõpule viia ja salvestada konfiguratsiooni katsel toiming nurjub. | Algsel installimisel veebiteenuse puhverserveri pole konfigureeritud, isegi kui seal on tegelik puhverserveri kohas. | Kasutage [Test-HcsmConnection cmdlet-käsk] [ 2] leidmiseks viga. Kui te ei saa selle probleemi lahendamiseks [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md) . |
|  4  | Autonoomsest HcsSetupWizard: Väärtus jääb vahemikku. | On vale alamvõrgu mask toodab selle tõrke. Võimalikud põhjused on järgmised. <ul><li> Alamvõrgu mask on tühi või puudub.</li><li>Ipv6 eesliite vorming on vale.</li><li>Kasutajaliideses on pilv lubatud, kuid lüüsi puuduva või vale.</li></ul>Pöörake tähelepanu sellele andmete 0 automaatselt pilve lubatud kui viisardi konfigureeritud. | Probleemi määratlemiseks kasutada alamvõrgu 0.0.0.0 või 256.256.256.256 ja seejärel vaadata väljund. Sisestage õigete väärtuste alamvõrgu mask, lüüsi ja Ipv6 eesliite, vastavalt vajadusele. |
 
## <a name="error-codes"></a>Tõrkekoodid

Vead on loetletud arvuline järjestus.

|Tõrkenumber|Tõrge teksti või kirjeldus|Soovitatav Kasutajatoiming|
|:---|:---|:---|
|10502|Salvestusruumi kontole juurdepääsemise ilmnes tõrge.|Oodake mõni minut ja proovige seejärel uuesti. Kui tõrge kordub, võtke ühendust Microsofti toega järgmised juhised.|
|40017|Varukoopia toiming nurjus, nagu määratud varukoopia poliitika maht ei leitud seadmes.|Proovige varukoopia toimingut, kui tõrge kordub, pöörduge Microsoft Support. järgmised juhised.|
|40018|Varukoopia toiming nurjus, nagu ükski täpsustatud varukoopia poliitika leiti seadmes. |Proovige varukoopia toimingut, kui tõrge kordub, pöörduge Microsoft Support. järgmised juhised.|
|390061|Süsteem on hõivatud või pole saadaval.|Oodake mõni minut ja proovige seejärel uuesti. Kui tõrge kordub, võtke ühendust Microsofti toega järgmised juhised.|
|390143|Tõrkekood 390143 ilmnes tõrge. (Tundmatu tõrge).|Kui tõrge kordub, pöörduge Microsoft Support järgmised toimingud.|

## <a name="next-steps"></a>Järgmised sammud

Kui te ei lahene, [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) abi saamiseks. 


[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
