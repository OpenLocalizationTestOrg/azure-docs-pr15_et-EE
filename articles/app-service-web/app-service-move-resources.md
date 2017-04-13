<properties
    pageTitle="Web App ressursside teisaldamine teise ressursirühm"
    description="Kirjeldatakse stsenaariumid, kus saate teisaldada Web rakenduste ja teenuste rakendus ressursi ühest rühmast teise."
    services="app-service"
    documentationCenter=""
    authors="ZainRizvi"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/04/2016"
    ms.author="zarizvi"/>
    
# <a name="supported-move-configurations"></a>Toetatud Teisalda konfiguratsioone

Azure'i veebirakenduse ressursside [ARM teisaldamine ressursid Api](../resource-group-move-resources.md)abil saate liikuda.

Azure'i veebirakenduste toetab praegu Teisalda järgmistel juhtudel:

* Ressursirühm (veebirakenduste, rakenduse teenuse lepingud ja serdid) kogu sisu teisaldamine teise ressursirühm 
    * Märkus: Sihtkoha ressursirühm võib olla mis tahes Microsoft.Web ressursid selle stsenaariumi
* Üksikute veebirakenduste teisaldamine erinevate ressursirühm ajal endiselt hosting neid oma praeguse rakenduse teenusleping (rakenduse teenusleping jääb vana ressursirühma)
