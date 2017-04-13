<properties
    pageTitle="Vastendage talletusruumi Azure saidi taastamise Hyper-V virtuaalse masina kopeerimise vahel kohapealse andmekeskuste | Microsoft Azure'i"
    description="Ettevalmistused salvestusruumi vastenduse Hyper-V virtuaalse masina kopeerimise vahel kahe kohapealse andmekeskuste Azure saidi taastamine."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/06/2016"
    ms.author="raynew"/>


# <a name="prepare-storage-mapping-for-hyper-v-virtual-machine-replication-between-two-on-premises-datacenters-with-azure-site-recovery"></a>Ettevalmistused salvestusruumi vastenduse Hyper-V virtuaalse masina kopeerimise vahel kahe kohapealse andmekeskuste Azure'i saidi taastamine


Azure'i saidi taastamine aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Selles artiklis kirjeldatakse salvestusruumi vastenduse, mis aitab teil optimaalselt kasutada salvestusruumi, kui kasutate saidi taastamine korrata Hyper-V virtuaalmasinates kahe kohapealse VMM andmekeskuste vahel.

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Ülevaade

Salvestusruumi vastenduse on oluline ainult siis, kui te olete imitatsiooniga Hyper-V virtuaalmasinates, mis asuvad VMM pilved esmane andmekeskuse teisene andmekeskusesse, kasutades Hyper-V koopia või SAN dispersioonanalüüs, järgmiselt:


- **Hyper-V koopia kohapealse kopeerimise abil kohapealse)** – Saate häälestada salvestusruumi vastenduse, vastenduse salvestusruumi liigitusi allika ja suunata VMM serverid teha järgmist:

    - **Tuvastamine target salvestusruum koopia virtuaalmasinates**– virtuaalmasinates saab paljundada salvestusruumi target (SMB ühiskasutusse anda või klaster ühiskasutusega mahud (CSV-de kaitse)) teie valitud.
    - **Koopia virtuaalse masina paigutuse**– salvestusruumi vastenduse kasutatakse optimaalselt paigutamiseks koopia virtuaalmasinates Hyper-V hosti serverites. Koopia virtuaalmasinates paigutatakse hosts, millele pääsete juurde vastendatud salvestusruumi liigitamine.
    - **Salvestusruumi kaardistamine**– kui konfigureerite ei salvestusruumi vastenduse, virtuaalmasinates kuvatakse kopeeritud talletamise vaikeasukohta määratud seostatud koopia virtuaalse masina Hyper-V hosti server.

- **Kohapealse SAN kohapealse kopeerimise abil**– häälestamise salvestusruumi vastenduse, vastenduse salvestusruumi massiivi kaustu luua ja suunata VMM serverites.
    - **Määrake pool**– määratleb, milliseid teisene talletusmahtu saab dispersioonanalüüs esmane pool.
    - **Tuvastamine target salvestusruumi kaustu**– tagab, et LUNs allika dispersioonanalüüs rühma ei kopeeritud vastendatud target talletusmahtu oma valik.

## <a name="set-up-storage-classifications-for-hyper-v-replication"></a>Salvestusruumi liigitusi Hyper-V kopeerimise häälestamine

Kui kasutate Hyper-V koopia saidi taastamine kopeerida, vastendate salvestusruumi liigitusi lähte- ja VMM serverites või ühes serveris VMM vahel, kui kaks saitide haldab sama VMM server. Pange tähele järgmist.

- Kui vastenduse on õigesti konfigureeritud ja kopeerimine on lubatud, virtuaalse masina virtuaalse kõvaketta esmane asukohta kopeeritud talletusruumi vastendatud sihtkoht.
- Salvestusruumi liigitusi peab olema host rühmad asuvad lähte- ja pilved saadaval.
- Liigitusi ei pea olema sama tüüpi salvestusruumi. Näiteks saate vastendada allika liigitamine, mis sisaldab SMB aktsiate target liigitamine, mis sisaldab CSV-de kaitse.
- Lugege lisateavet, kuidas [luua salvestusruumi liigitusi VMM](https://technet.microsoft.com/library/gg610685.aspx).

## <a name="example"></a>Näide

Kui liigitusi on õigesti konfigureeritud VMM lähte- ja sihtsaitide VMM serveri valimisel salvestusruumi vastendamisel, kuvatakse lähte- ja liigitusi. Siin on näide salvestusruumi failide osakud ja liigitusi kahes kohas New York ja Chicago organisatsiooni jaoks.

**Asukoht** | **VMM server** | **Faili ühiskasutus (allikas)** | **Liigitamine (allikas)** | **Vastendatud** | **Faili ühiskasutus (sihtrentnik)**
---|---|--- |---|---|---
New York | VMM_Source| SourceShare1 | KULD | GOLD_TARGET | TargetShare1
 |  | SourceShare2 | HÕBE | SILVER_TARGET | TargetShare2
 | | SourceShare3 | PRONKS | BRONZE_TARGET | TargetShare3
Chicago | VMM_Target |  | GOLD_TARGET | Pole vastendatud |
| | | SILVER_TARGET | Pole vastendatud |
 | | | BRONZE_TARGET | Pole vastendatud

Seadistada need vahekaardil **Server salvestusruum** **ressursid** lehel saidi taastamine portaali.

![Konfigureerige salvestusruumi vastendamine](./media/site-recovery-storage-mapping/storage-mapping1.png)

See näide:
- Kui soovitud koopia virtuaalse masina luuakse mis tahes virtuaalse masina KULDMEDALI salvestusruumi (SourceShare1), see on kopeeritud GOLD_TARGET salvestusruumi (TargetShare1).
- Koopia virtuaalse masina loomisel hõbedane salvestusruumi (SourceShare2) mis tahes virtuaalse masina paljundada SILVER_TARGET (TargetShare2) salvestusruumi ja nii edasi.

Tegelik failikettad ja nende määratud liigitusi VMM kuvatakse järgmine ekraanipilt.

![Salvestusruumi liigitusi VMM](./media/site-recovery-storage-mapping/storage-mapping2.png)

## <a name="multiple-storage-locations"></a>Mitme salvestusruumi asukohad

Kui target liigitamine on määratud mitme SMB aktsiad või CSV-de kaitse, optimaalse talletuskoht valitakse automaatselt, kui virtuaalse masina on kaitstud. Kui pole sobiv target salvestusruumi on saadaval määratud liigitamine, kasutatakse määratud Hyper-V hosti talletamise vaikeasukohta koopia virtuaalse kõvaketta viia.

Järgmises tabelis kuvatakse, kuidas salvestusruumi liigitamine ja ühiskasutuses kobar maht on häälestatud siinses näites.

**Asukoht** | **Liigitamine** | **Seotud salvestusruum**
---|---|---
New York | KULD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p>
 | HÕBE | <p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p>
Chicago | GOLD_TARGET | <p>C:\ClusterStorage\TargetVolume1</p><p>\\FileServer\TargetShare1</p>
 | SILVER_TARGET| <p>C:\ClusterStorage\TargetVolume2</p><p>\\FileServer\TargetShare2</p>

Selles tabelis on kokkuvõte käitumise kui lubate kaitse virtuaalmasinates (VM1 - VM5) selles näites keskkonnas.

**Virtuaalse masina** | **Andmeallika salvestusruum** | **Andmeallika liigitamine** | **Vastendatud target salvestusruum**
---|---|---|---
VM1 | C:\ClusterStorage\SourceVolume1 | KULD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\\FileServer\SourceShare1</p><p>Mõlemad GOLD_TARGET</p>
VM2 | \\FileServer\SourceShare1 | KULD | <p>C:\ClusterStorage\SourceVolume1</p><p>\\FileServer\SourceShare1</p> <p>Mõlemad GOLD_TARGET</p>
VM3 | C:\ClusterStorage\SourceVolume2 | HÕBE | <p>C:\ClusterStorage\SourceVolume2</p><p>\FileServer\SourceShare2</p>
VM4 | \FileServer\SourceShare2 | HÕBE |<p>C:\ClusterStorage\SourceVolume2</p><p>\\FileServer\SourceShare2</p><p>Mõlemad SILVER_TARGET</p>
VM5 | C:\ClusterStorage\SourceVolume3 | N/A | Kaardistamine, seega kasutatakse talletamise vaikeasukohta Hyper-V hosti

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil on salvestusruumi vastendamise [ettevalmistamine juurutada Azure saidi taastamise](site-recovery-best-practices.md)paremini mõista.
