<properties
   pageTitle="Azure teenuse struktuuri Linux | Microsoft Azure'i"
   description="Teenuse struktuuri kogumite toeta Linux ja Java, mis tähendab, et teil võimalik juurutada ja host teenuse struktuuri taotluste kirjutatud Java ja C# Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Teenuse struktuuri Linux

Teenuse struktuuri Linux eelvaade võimaldab teil koostada, juurutada ja hallata väga kättesaadav, väga paindlik rakendusi Linux, nagu teeksite Windows. Teenuse struktuuri raamistiku (usaldusväärsed teenused ja usaldusväärne osalejate) on saadaval Java Linux lisaks C# (.NET tuum).  Samuti saate koostada [käivitatava teenused](service-fabric-deploy-existing-app.md) keel või raames. Lisaks toetab eelvaate orkesteroinnin keskmise suurusega ümbriste. Keskmise suurusega ümbriste saab käitada Külastajate täitmisfailid või kohalikke teenuse struktuuri teenuse kasutamisel teenuse struktuuri raamistiku.

Teenuse struktuuri Linux võrdub põhimõtteliselt teenuse struktuuri Windows (v.a OS üksikasjad ja programmeerimisega keelte tugi). Seega kehtib enamiku oma [olemasolevad dokumendid](http://aka.ms/servicefabricdocs) aidata teil saate tutvuda tehnoloogia.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Toetatud opsüsteemid ja programmeerimise keeled

Piiratud eelvaade toetab ühe-karbi arengu kogumite Lisaks mitme masina kogumite loomine Azure Ubuntu Server 16.04 töötab. Eelvaate toetab lisaks Külastajate Käivitusfailid ja keskmise suurusega ümbriste orkesteroinnin Java ja C# usaldusväärne osalejate ning usaldusväärne kodakondsuseta teenuste raamistiku.  

>[AZURE.NOTE] Usaldusväärne saidikogumid ei toetata Linux veel. Seisma eraldi kogumite ei toetata – kas ainult üks kast ja Azure Linux mitme masina kogumite on toetatud eelvaates.

## <a name="supported-tooling"></a>Toetatud instrumentaarium

Eelvaate toetab kobar Azure'i CLI kaudu suhtlemine. Java arendajatele, Eclipse ja Yeoman integreerimine on varustatud Eclipse toetatud Linux ja OSX. OSX integreerimine kasutab Linux VM kaitstud vagrant kaudu. C# arendajatele, Yeoman integreerimine on esitatud Mallid luua.

## <a name="next-steps"></a>Järgmised sammud


1. Saate tutvuda [Usaldusväärne osalejad](service-fabric-reliable-actors-introduction.md) ja [Usaldusväärsed teenused](service-fabric-reliable-services-introduction.md) programmeerimise raamistiku.

2. [Teie arenduskeskkond Linux ettevalmistamine](service-fabric-get-started-linux.md)

3. [Teie arenduskeskkond OSX ettevalmistamine](service-fabric-get-started-mac.md)

4. [Linux esimese teenuse struktuuri Java rakenduse loomine](service-fabric-create-your-first-linux-application-with-java.md)
