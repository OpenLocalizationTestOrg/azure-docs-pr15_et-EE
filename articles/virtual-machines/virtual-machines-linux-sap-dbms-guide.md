<properties
   pageTitle="SAP NetWeaver kohta Linux virtuaalmasinates (VMs) – DBMS juurutusjuhend | Microsoft Azure'i"
   description="SAP NetWeaver Linux virtuaalmasinates (VMs) – DBMS juurutusjuhend kohta"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>SAP NetWeaver Azure'i virtuaalmasinates (VMs) – DBMS juurutusjuhend kohta

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver kohta Linux virtuaalmasinates (VMs) – DBMS juurutusjuhend) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (vahemälu VMs ja VHDs) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (tarkvara RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure'i Tabelimäluga) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (RDBMS juurutamise ülesehitus) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (suur-saadavus ja Õnnetusejärgne taastamine Azure'i VMs) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 ja uuemad versioonid) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 ja varasemates versioonides) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (kasutades SQL serveri pildid välja Microsoft Azure'i turuplatsi) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (üldine SQL Server Azure'i Kokkuvõte SAP-i) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (üksikasjad RDBMS SQL serveri abil) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (salvestusruumi konfiguratsioon) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (varundus ja taaste) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (jõudluse kaalutluste kohta varundus ja taaste) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (muu) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver Linux virtuaalmasinates (VMs) – juurutusjuhend kohta) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP-i ressursid) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (juurutamine kohandatud pildiga VM) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (stsenaarium 1: juurutamine VM välja Azure'i turuplatsi SAP) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (stsenaarium 2: juurutamine VM kohandatud pildiga SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 () Stsenaarium 3: VM liikudes kohapealse-üldiste Azure'i VHD SAP-i abil) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (juurutamise stsenaariumid, VMs SAP Microsoft Azure) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (juurutamine Azure PowerShelli cmdletid) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Laadi alla ja impordi SAP-i jaoks oluline PowerShelli cmdlet-käskude) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (liitumine VM üheks kohapealse domeeni - ainult Windows) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [juurutus-juhend-4.4]: Virtual-Machines-Linux-SAP-Deployment-Guide.MD#c7cbb0dc-52a4-49dB-8e03-83e7edc2927d (allalaadimine, installimine ja luba Azure VM Agent) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShelli) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure'i CLI) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (konfigureerimine Azure'i täiustatud jälgimine laiend SAP-i) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (valmisoleku kontrollimiseks Azure'i täiustatud jälgimine SAP-i) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (seisundikontrolli Azure Jälgimine taristu konfiguratsiooni) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (täpsemaks tõrkeotsing Azure'i jälgimise infrastruktuuri SAP-i)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver Linux virtuaalmasinates (VMs) – kavandamisel ja rakendamise juhendi kohta) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (ressursid) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (kõrge kättesaadavus (HA) ja katastroofi taastamine (DR) SAP NetWeaver töötab Azure'i Virtuaalmasinates) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (kõrge kättesaadavus SAP-i rakenduste serverid) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (kasutades automaatselt käivitada ka SAP-i eksemplaride) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (vaid - virtuaalse masina juurutuste Azure'i sisse ilma kohapealse kliendi võrgu sõltuvuse) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (rist – eeldusel - juurutamise ühe või mitme SAP-i VMs Azure'i täitmiseks täielikult integreeritud kohapealse võrku sisse) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure'i piirkonnad) [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (viga domeenid) [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (täiendamine domeenide) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure'i kättesaadavus komplekti) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (The Microsoft Azure virtuaalse masina mõiste) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (liikumine VM kohapealse Azure-üldiste kettal) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (juurutamine pildiga klientide teatud VM) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (ettevalmistamine teisaldamine VM kohapealse Azure-üldiste kettal) [ Planning-Guide-5.2.2]:Virtual-Machines-Linux-SAP-Planning-Guide.MD#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (ettevalmistamine juurutamine VM pildiga klientide teatud SAP) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (SAP Azure VMs ettevalmistamine) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (erinevuse vahel an Azure'i kettale ja Azure pilt) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (üleslaadimine on VHD asutusesisesest Azure) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (Azure'i salvestusruumi kontode vahel kopeerimine ketast) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD struktuur SAP-i juurutuste) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (säte automount jaoks manustatud ketast) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (ühe VM abil SAP NetWeaver demo/koolitus stsenaarium) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (SAP-i eksemplari kasutuselevõtt põhimõtet, Cloud-Only) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure'i jälgimise lahendus SAP-i) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium Storage)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.

Sellest juhendist leiavad osa dokumendist ja SAP-i tarkvara Microsoft Azure juurutamine. Enne selle juhendit lugeda, lugege [kavandamisel ja rakendamise juhend] [-plaanimisjuhend]. Selles dokumendis antakse ülevaade eri relatsiooniline andmebaasi süsteemid (RDBMS) ja sellega seotud toodetes SAP-i kohta Microsoft Azure'i Virtuaalmasinates (VMs) koos Azure infrastruktuuri teenus (IaaS) võimaluste kasutamine.

Paberi täiendab SAP-i installimise dokumentatsiooni ja SAP-i märkmeid, mis moodustavad esmane ressursside installid ja SAP-i tarkvara juurutuste kohta antud platvormid

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Üldised kaalutlused
See peatükk seotud kaalutluste käitamise SAP-i DBMS süsteemide Azure VMs tuuakse. On mõne kindla DBMS süsteemide selle peatüki viited. Selle asemel teatud DBMS süsteemide käsitlemist jooksul selle paberi pärast selle peatükk.

### <a name="definitions-upfront"></a>Määratlused algusest peale
Kogu dokumendi kasutame järgmiselt:

* IaaS: Taristu teenust.
* PaaS: Platvorm teenust.
* SaaS: Tarkvara kui teenus.
* SAP-i komponent: üksikute SAP-i rakendus nagu ECC, BW, lahendus Manager või EP.  SAP-i komponentide põhjal traditsiooniline ABAP või Java tehnoloogiad või näiteks äriobjektide vastavalt NetWeaver-rakendus.
* SAP-i keskkond: üks või mitu SAP-i komponendid loogiliselt rühmitatud business funktsiooni arengu, QAS, koolitus, DR või tootmise sooritamiseks.
* SAP-i horisontaalpaigutus: See viitab kogu SAP-i varade kliendi selle horisontaalpaigutust. SAP-i horisontaalpaigutus hõlmab kõiki tootmise ja mitte tekitamiseks keskkonnas.
* SAP-i süsteemis: Kombinatsioon DBMS kiht ja kihid, nt mõni SAP ERP arendamise süsteemi, SAP-i kohta testi süsteemi, SAP CRM süsteem jne. Ei toetata Azure juurutuste jagamine need kaks kihti kohapealse ja Azure vahel. See tähendab, et SAP süsteemi on kas juurutatud kohapealse või Azure juurutamist. Siiski saate juurutada erinevate süsteemid on SAP-i maastiku Azure'i või kohapealse. Näiteks võib juurutada SAP CRM arendamise ja Azure, kuid SAP CRM tootmise süsteemi asutusesisese süsteemide kontrollimiseks.
* Vaid juurutamise: juurutamine, kus pole ühendatud Azure tellimuse-saidilt või kohapealse võrgu taristule ExpressRoute ühenduse kaudu. Ühine Azure dokumentatsiooni juurutuste sellised on ka kirjeldatud "Ainult pilve" kasutuselevõttu. Selle meetodi juurutatud Virtuaalmasinates pääseb Interneti kaudu ja avaliku Interneti lõpp-punktid määratud Azure'i VM. Asutusesisene Active Directory (AD) ja DNS-i ei laiene enam järgmist tüüpi juurutuste Azure. Seega ei ole VMs kohapealse Active Directory osa. Märkus: Vaid juurutuste selles dokumendis on määratletud täieliku SAP-i maastiku, mis töötab ainult Azure Active Directory või nimelahendus laiendita asutusesisesest avaliku pilv. Vaid konfiguratsioone ei toetata tootmissüsteemide SAP-i või konfiguratsioone, kus SAP-i spetsiaalsete Andmeedastusmoodulite või muud ressursid kohapealse tuleb kasutada majutatud Azure ja ressursside elavate kohapealse SAP-i süsteemi vahel.
* Asutusesiseses: Kirjeldatakse stsenaarium, kus on juurutatud Azure'i tellimus, mis sisaldab-saidilt, mitme saidi või ExpressRoute Ühenduvus kohapealse datacenter(s) ja Azure VMs. Ühine Azure dokumentatsiooni kohta, juurutuste sellised on kirjeldada asutusesiseses stsenaariumid. Ühenduse põhjus on kohapealse domeenide laiendada, kohapealse Active Directory ja kohapealse DNS-i Azure'i sisse. Azure'i vara tellimus pikendatakse kohapealse horisontaalpaigutus. Millel see laiend, saab VMs kohapealse domeeni osa. Kohapealse domeeni domeeni kasutajad pääseksid serverid ja käivitada teenuste nende VMs (nt DBMS teenused). VMs vahelise suhtluse ja nimi eraldusvõime juurutatud kohapealse ja juurutada Azure VMs on võimalik. Loodame, et see on kõige levinum stsenaarium SAP-i varad Azure kasutamise kohta. Vaadake [seda] [ vpn-gateway-cross-premises-options] artiklis ja [selle] [ vpn-gateway-site-to-site-create] lisateabe saamiseks.

> [AZURE.NOTE] SAP-i tootmissüsteemide asutusesiseses juurutuste SAP-i süsteemide, kus töötab SAP-i süsteemide Azure'i Virtuaalmasinates on liikmete kohapealse domeeni on toetatud. Asutusesiseses konfiguratsioone on toetatud juurutamine osade või täitke SAP-i maastiku Azure'i sisse. Isegi töötab täieliku SAP-i horisontaalpaigutus Azure nõuab on need VMs kohapealse domeeni ja reklaamid osana. Endise versioonides dokumentatsiooni rääkisime hübriid-IT stsenaariumid, kus Termini 'Hübriid' juured on asjaolu, et on asutusesiseses Ühenduvus kohapealse ja Azure. Sel juhul tähendab "Hübriid", Azure'i VM kuuluvad kohapealse Active Directory.

Mõned Microsoft dokumentatsioonis kirjeldatakse asutusesiseses stsenaariumid veidi erineda, eriti jaoks DBMS HA konfiguratsioone. SAP puhul seotud dokumendid asutusesiseses stsenaarium lihtsalt paised allapoole võttes-saidilt või privaatne (ExpressRoute) Ühenduvus ja SAP-i horisontaalpaigutus, jagatud kohapealse ja Azure asjaolu.

### <a name="resources"></a>Ressursid
Järgmised kasutusevõtuks on saadaval, SAP-i juurutuste Azure teema.

* [SAP NetWeaver Azure'i virtuaalmasinates (VMs) – kavandamisel ja rakendamise juhendi kohta] [-plaanimisjuhend]
* [SAP NetWeaver Azure'i virtuaalmasinates (VMs) – juurutusjuhend kohta] [juurutusjuhend]
* [SAP NetWeaver Azure'i virtuaalmasinates (VMs) – DBMS juurutusjuhend (see dokument) kohta] [dbms Tutvustus]

SAP-i järgmises märkmed on seotud SAP-i Azure teema:

| Märkus arv   | Pealkiri
|------------|--------
| [1928533] | SAP-i rakenduste Azure: toetatud tooted ja Azure VM tüübid
| [2015553] | SAP-i Microsoft Azure: toetavad eeltingimused
| [1999351] | Täiustatud Azure jälgimise SAP-i tõrkeotsing
| [2178632] | Klahv SAP-i Microsoft Azure mõõdikute jälgimine
| [1409604] | Opsüsteemis Windows Virtualization: põhjalikuma
| [2191498] | SAP-i Linux ja Azure: põhjalikuma
| [2039619] | SAP-i rakenduste Microsoft Azure abil Oracle'i andmebaasiga: toetatud tooted ja versioonid
| [2233094] | DB6: SAP-i rakenduste Azure Linux, UNIX ja Windows – täiendavat teavet IBM DB2 abil
| [2243692] | Microsoft Azure'i (IaaS) VM Linuxi: SAP-i litsentsi probleemid
| [1984787] | SUSE LINUX Enterprise Server 12: Installimärkmed
| [2002167] | Punane rolli Enterprise Linuxi 7.x: installimine ja täiendamine

Lugege [SCN viki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) sisaldava märkmed SAP-i Linuxi jaoks.

Microsoft Azure'i arhitektuur ja kuidas Microsoft Azure'i Virtuaalmasinates juurutatud ja, mida käitab tundma peaks teil olema. Lisateavet leiate siit <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Me **ei** käsitleta Microsoft Azure'i platvormi nimega Service (PaaS) pakkumisi Microsoft Azure'i platvormi. See raamat räägib töötab andmebaasi halduse süsteemi (DBMS) rakenduses Microsoft Azure'i Virtuaalmasinates (IaaS), nagu sooviksite käivitate selle DBMS kohapealse keskkonna. Andmebaasi võimaluste ja nende kahe pakkumiste vahel funktsioone on väga erinevad ja ei tohi segada üksteisega. Vt ka: <https://azure.microsoft.com/services/sql-database/>

Kuna me ei käsitleta IaaS, üldiselt Windowsi, Linuxi ja DBMS installimine ja konfigureerimine on põhiosas sama, mis tahes virtual või installite kohapealse tühjal metalliga taustaga arvuti. Kuid on mõned arhitektuur ja süsteemi halduse rakendamist otsuste teistsugused kui IaaS kasutades. Selle dokumendi eesmärki on selgitada konkreetseid arhitektuuri ja süsteemi haldus erinevused, mida peate olema valmis jaoks IaaS kasutamisel.

Üldiselt, on üldised alade poolest, et arutada, st:

* Proper VM/VHD paigutuse SAP-i süsteemide, et teil oleks õige andmete plaanimise faili paigutus ja saavutada piisavalt IOPS jaoks oma töökoormus.
* Võrgunduse kaalutlused IaaS kasutamisel.
* Kindla andmebaasi funktsioone kasutada selleks, et optimeerida andmebaasi paigutus.
* Varundus ja taaste kaalutlused IaaS.
* Kasutades erinevat tüüpi pilte juurutamiseks.
* Azure'i IaaS kõrge saadavus.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktuur RDBMS juurutamine
Selleks järgige selles Peatükk, on vaja mõista, mis on esitatud [selle] [juurutus-juhend-3] Peatükk [kasutuselevõtu juhend] [juurutusjuhend]. Teadmisi erinevate VM sarja ja nende erinevusi ja vahede Azure'i Standard ja Premium salvestusruumi tuleks käsitleda ja teada enne selle peatüki lugemist.

Märts 2015, kuni Azure'i VHDs, mis sisaldavad operatsioonisüsteem on piiratud 127 GB suuruseid. Seda piirangut sai tõstetakse märts 2015 (kohta rohkem teavet sisse <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ). Seal VHDs sisaldav operatsioonisüsteem on sama suur kui mis tahes muud VHD. Siiski me siiski eelistan struktuuri juurutus, kus operatsioonisüsteemi, DBMS ja lõpuks SAP-i kahendfaile on eraldi andmebaasi faile. Seetõttu loodame SAP-i opsüsteemi Azure'i Virtuaalmasinates on base VM (või VHD) installitud operatsioonisüsteemi andmebaasi halduse süsteemi täitmisfailid ja SAP-i executables. DBMS andmete ja logide failid salvestatakse Azure Storage (Standard- või Premium salvestusruumi) eraldi VHD failid ja lisatud loogilise ketast Azure operatsioonisüsteemi Originaalpilt VM. 

Sõltub kasutamine Azure Standard- või Premium salvestusruumi (nt, kasutades DS-sari ja GS-sarja VMs) seal on muud kvootide Azure'i, mis on [siin][virtual-machines-sizes]. Kui plaanite oma Azure'i VHDs, peate parim saldo kvootide järgmist.

* Andmete failide arvu.
* VHDs, mis sisaldavad failide arvu.
* Ühe VHD IOPS kvootide.
* Andmete läbilaskevõime VHD kohta.
* Täiendavad VHDs võimalik per VM suurus arv.
* Üldine salvestusruumi jõudlus VM saate sisestada.
 
Azure'i on jõustada ka IOPS kvoodi kohta VHD ketas. Nende kvootide on erinevad VHDs majutatud Azure'i Standard ja Premium. I/O latentsused on ka salvestusruumi ühelt Premium Storage pakkuda tegureid parem I/O latentsused oluliselt erinev. Iga VM tüüpide on piiratud arv VHDs, mida on võimalik lisada. Teine piirang on, et ainult teatud VM saate kasutada Azure Premium Storage. See tähendab, et otsust VM kindlat tüüpi jaoks võib mitte ainult juhtida CPU ja mälu nõuetele, vaid ka IOPS, latentsus ja ketta tavaliselt mastaabitud VHDs arv või tüüp Premium salvestusruumi ketast läbilaskevõime nõuete. Puhul, mis sisaldavad Premium mälu on VHD suurust ka võib olla tingitud IOPS ja läbilaskevõime, mis tuleb iga VHD saavutada arv.

Asjaolu, et üldine IOPS määr, VHDs arvu paigaldatud ja VM mahu kõik omavahel seotud, võivad põhjustada SAP-i süsteemi erinema oma kohapealse juurutuse Azure konfigureerimisel. IOPS limiitide kohta LUN on tavaliselt konfigureeritav kohapealse kasutuselevõttu. Azure Storage need piirangud on kinnitatud või nagu Premium mälu sõltub ketta tüüp. Nii kohapealse juurutuse näeme kliendi konfiguratsioone andmebaasi serverites, mis kasutavad palju erinevaid mahtu teisiti täitmisfailid nagu SAP-i ja DBMS või teisiti mahud ajutine andmebaasid või tabeli tühikuid. Kui kohapealse süsteemi teisaldatakse Azure'i võib kaasa tuua võimalikud IOPS läbilaskevõime raiskamine, raiskad on VHD täitmisfailid või andmebaase, mida teha, kõik või mitte palju IOPS. Azure'i VM soovitame DBMS ja SAP-i täitmisfailid olema installitud OS kettal, kui võimalik.

Andmebaasi faile ja logifailid ja Azure Storage kasutanud, tüüpi paigutuse peaks olema määratletud IOPS, latentsus ja läbilaskevõime nõuete. Selleks on piisavalt IOPS tehingulogi jaoks, mida võib kasutajanimeks kasutada mitut VHDs tehingulogi faili jaoks või kasutada suuremat Premium salvestusruumi ketast. Sellisel juhul ühte lihtsalt luua RAID tarkvara (nt Windows salvestusruumi rakenduskausta for Windows või MDADM ja LVM (loogika helitugevuse Manager) Linuxi) koos VHDs, mis sisaldab tehingulogi.

___

> ![Windows][Logo_Windows] Windows
>
> Ketas D:\ rakenduses on Azure VM on jätkunud ketas, mis on tagatud mõne kohaliku ketta Azure MSDN. Kuna tegemist on säilis, see tähendab, et sisu D:\ kettal, et tehtud muudatused on kaotsi, kui VM taaskäivitatakse. "Muutused", tähendab salvestatud failide kataloogid loodud, installitud rakendused, jne.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure VMs automaatselt draivi ühendamiseks /mnt/resource, mis toetab kohaliku ketast Azure MSDN-säilis kettal on veebisaidil. Kuna tegemist on säilis, see tähendab, et sisu /mnt/resource tehtud muudatused on kaotsi, kui VM taaskäivitatakse. Muudatused, mida tähendab salvestatud failide, kataloogid, mis on loodud, installitud rakendused, jne.

___

Sõltuvad Azure VM sarja kohaliku ketast Arvuta sõlme Kuva eri jõudluse, mida saab liigitada, nt:

* A0 – A7: Väga vähe jõudlust. Midagi Lisaks windows lehe faili ei saa kasutada
* A8-A11: Mõned 10 000 IOPS väga head jõudlust omadusi ja > 1GB sekundis läbilaskevõime
* D-sarja: Mõned 10 000 IOPS väga head jõudlust omadusi ja > 1GB sekundis läbilaskevõime
* DS-sarja: Mõned 10 000 IOPS väga head jõudlust omadusi ja > 1GB sekundis läbilaskevõime
* G-sarja: Mõned 10 000 IOPS väga head jõudlust omadusi ja > 1GB sekundis läbilaskevõime
* GS-sarja: Mõned 10 000 IOPS väga head jõudlust omadusi ja > 1GB sekundis läbilaskevõime

Ülaltoodud laused on rakendamine VM tüübid, mis on serditud SAP-i abil. VM sarja suurepäraseid IOPS ja läbilaskevõime saada võimendada DBMS funktsioone, nagu tempdb või ajutise tabeli ruumi.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Vahemällu VMs ja VHDs
Kui nende ketast/VHDs portaali kaudu või me mount üleslaaditud VHDs vms koostamiseks me saate valida, kas vahemälus talletatud I/O liikluse VM ja need asuvad Azure storage VHDs vahel. Azure'i Standard- ja Premium mälu kaks erinevate tehnoloogiatega seda tüüpi vahemälu. Mõlemal juhul ise vahemälu oleks sama kaudu kasutatavaid VM ajutise kettaruumi (D:\ Windows) või /mnt/resource Linuxi toetada kettale.
 
Azure'i Standard Storage võimalike vahemälu on:

* Pole vahemällu
* Lugege vahemällu talletamine
* Lugemine ja kirjutamine vahemällu talletamine

Selleks, et saada järjepidevaid ja tarkadeks jõudluse, on soovitatav määrata vahemällu Azure Standard Storage for kõik VHDs sisaldav **DBMS seotud andmefailid, logifailid ja tabeli ruumi 'NONE'**. Vahemällu VM võib jääda vaikimisi.

Azure Premium Storage olemas vahemällu järgmistest suvanditest:

* Pole vahemällu
* Lugege vahemällu talletamine

Soovitus Azure Premium Storage on võimendada, **Lugege vahemällu andmefailide** SAP-i andmebaasi ja valitud **pole vahemällu VHD-faile, logifailid**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Tarkvara RAID
Nagu juba eespool, peate jääk arvu IOPS vaja andmebaasi faile saate konfigureerida VHDs arv ja tippige maksimaalne IOPS, mis on Azure VM annab VHD või Premium salvestusruumi kettale kohta. Lihtsaim viis tegeleda IOPS laadi üle VHDs on üle erinevate VHDs tarkvara RAID koostamiseks. Seejärel hulgaliselt andmefailid, SAP-i DBMS uuristatud tarkvara RAID LUNs. Sõltuvad nõuded, võiksite kaaluda ka kaks kolm alates Premium mälu kasutamist erinevate Premium salvestusruumi ketast pakuvad kõrgema IOPS kvoodi kui VHDs kindlad põhjal. Lisaks märgatavalt parem I/O latentsus esitatud Azure Premium Storage. 

Sama kehtib tehingulogi erinevat DBMS süsteemi. Palju need lihtsalt rohkem Tlog failide lisamine ei aita, kuna DBMS süsteemide kirjutamine korraga ainult ühte failid. IOPS kõrgem kui ühe Standard salvestusruumi vastavalt vajadusel VHD suudab, te saate triip üle mitme Standard salvestusruumi VHDs või suurem Premium salvestusruumi ketta tüüpi, mis lisaks suuremat IOPS ka pakub tegureid alumise latentsus kirjutamine väljundtoimingute tehingulogi sisse saate kasutada.
 
Olukorrad, mis on Azure juurutuste, millele soovite kasuks, kasutades tarkvara kogenud RAID on:

* Log/uuestitegemine tehingulogi vaja IOPS rohkem kui ühe VHD pakub Azure. Nagu eespool kirjeldatud seda saab lahendada hoone a LUN üle mitme VHDs kasutades tarkvara RAID.
* Ebaühtlane I/O töökoormus jaotuse üle erinevate andmefailid SAP-i andmebaasist. Sellisel juhul võib kogemuse pihta kvoodi pigem sageli ühe andmefaili. Samas muid andmeid faile ei saa isegi ühe VHD IOPS piirmäära lähedale. Lihtsaim lahendus on sellisel juhul koostada ühe LUN üle mitme VHDs kasutades tarkvara RAID. 
* Te ei tea täpset I/O töökoormus andmefaili on ja ainult umbes teada, mis on üldine IOPS töökoormus on DBMS vastu. Kõige lihtsam teha on üks LUN tarkvara RAID abil luua. Mitme VHDs taha seda LUN kvootide summa siis tuleks täita teadaolevad IOPS määr.

___

> ![Windows][Logo_Windows] Windows
>
> Windows Server 2012 või uuem versioon salvestusruum kasutamist on soovitatav, kuna see on tõhusam Windows triibustus Windowsi varasemate versioonide. Pidage meeles, et peate Windowsi salvestusruumi kaustu ja salvestusruum loomine PowerShelli käske, kui kasutate operatsioonisüsteemi Windows Server 2012. PowerShelli käske leiate siit <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Tarkvara RAID Linux koostamiseks toetatakse ainult MDADM ja LVM (loogika helitugevuse Manager). Lisateabe saamiseks lugege järgmisi artikleid:
>
> * [Konfigureerida tarkvara RAID Linux] [ virtual-machines-linux-configure-raid] (MDADM) jaoks
> * [Linux VM Azure LVM konfigureerimine][virtual-machines-linux-configure-lvm]


___

Kaalutlused tõhustamiseks VM sari, mis on tavaliselt Azure Premium Storage töötamiseks on:

* I/O latentsused, mis SAN/NAS seadmeid esitamisel lähedal asuvate nõudmistele.
* Tegurid parem I/O latentsus kui Azure Standard salvestusruumi suudab järele.
* Suurema IOPS kui, mida on võimalik saavutada mitme Standard salvestusruumi VHDs teatud VM tüüpide kohta VM.

Kuna aluseks Azure Storage tiražeerib iga VHD vähemalt kolm salvestusruumi sõlmed, lihtsa RAID 0 striping saab kasutada. Ei ole vaja RAID5 või RAID1 rakendada.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure'i salvestusruum
Microsoft Azure'i Tabelimäluga talletada base VM (koos OS) ja VHDs või plekid vähemalt 3 eraldi salvestusruumi sõlmed. Salvestusruumi konto loomisel on valik kaitse, nagu on näidatud siin.

![Azure Storage konto jaoks lubatud Geo-Dispersioonanalüüs][dbms-guide-figure-100]

Azure'i salvestusruumi kohaliku Dispersioonanalüüs (kohalik üleliigsed) pakub andmete kaotsimineku taristu tõttu, et mõned kliendid võiks endale juurutada kaitse taset. Nagu eespool näidatud, viienda on üks kolmest esimene variatsioon on 4 erinevaid võimalusi. Kas soovite neid Täpsem me saame eristada:

* **Premium kohalikult liigsete salvestusruumi (LRS)**: Azure Premium Storage pakub suure jõudlusega, latentsusajaga ketta tugi virtuaalmasinates töötab I/O-nõudvaid töökoormus. On sama, mis on Azure piirkond Azure andmekeskuse asuvate andmete 3 koopiad. Eksemplarid on mõeldud muu viga ja täiendamine domeenide (põhimõtet vt [see] [kavandamise-juhend-3,2] peatüki [kavandamise Guide][planning-guide]). Andmete salvestamise sõlm tõrge või ketta tõrke tõttu läheb koopia korral luuakse automaatselt uue koopia.
* **Kohalik liigsete salvestusruumi (LRS)**: sel juhul on sama, mis on Azure piirkond Azure andmekeskuse asuvate andmete 3 koopiad. Eksemplarid on mõeldud muu viga ja täiendamine domeenide (põhimõtet vt [see] [kavandamise-juhend-3,2] peatüki [kavandamise Guide][planning-guide]). Andmete salvestamise sõlm tõrge või ketta tõrke tõttu läheb koopia korral luuakse automaatselt uue koopia. 
* **Geo liigsete salvestusruumi (GRS)**: sel juhul on asünkroonne dispersioonanalüüs, mis kuvatakse kanali mõne teise Azure'i piirkond, mis on enamikel juhtudel geograafilise piirkonna (nt Põhja- ja Lääne Euroopa) andmete täiendavad 3 koopiad. Selle tulemuseks 3 täiendavate koopiad, nii, et summa on 6 koopiad. See muudatus on lisaks kus saab kasutada andmete geo tiražeeritud Azure piirkonna loetuks eesmärgil (lugemisõigus – geograafilise liigne).
* **Tsooni liigsete mälu (ZRS)**: sel juhul 3 koopiad andmed jäävad Azure'i piirkonna. Nagu on selgitatud [see] [kavandamise-juhend-3,1], [Plaanimisjuhendi] [-plaanimisjuhend] Azure piirkond Peatükk võib olla arv andmekeskuste vahetus. LRS puhul selle koopiad jagamine eri andmekeskuste, mis muudavad ühe Azure piirkonna üle.

Lisateavet leiate [siit][storage-redundancy].
 
> [AZURE.NOTE] DBMS juurutuste korral on soovitatav Geo liigsete salvestusruumi kasutuse
>
> Azure'i salvestusruumi Geo-kopeerimine on asünkroonne. Lukusta etapis ei sünkroonita kopeerimine üksikute VHDs paigaldatud ühe VM. Seetõttu ei sobi jaotatud erinevate VHDs või juurutatud vastu tarkvara RAID alusel mitu VHDs DBMS failide kopeerida. DBMS tarkvara nõuab püsivate kettaruumi täpselt sünkroonitakse kogu muu LUNs ja aluseks ketast/VHDs/spindlid. DBMS tarkvara kasutab eri võimaldada jada IO kirjutamine tegevuste ja DBMS aru, et ketta talletamist suunatud soovitud dispersioonanalüüs on rikutud, kui need erinevad isegi mõne millisekundiga. Seega kui tõesti andmebaasi konfigureerimine kogu mitme VHDs geo tiražeeritud andmebaasi, nt dispersioonanalüüs peab toimuma andmebaasi tähendab ja funktsionaalne. Üks ärge klõpsake Azure salvestusruumi Geo-Dispersioonanalüüs seda tööd teha. 
>
> Probleem on kõige lihtsam näide süsteem selgitamiseks. Oletame, et teil on SAP süsteemi Azure'i, mis on 8 VHDs andmefailid on DBMS pluss üks VHD, mis sisaldab tehingulogi faili sisaldava faile üles laadida. Need 9 VHDs igat on andmete kirjutatud neile ühtse meetodi DBMS, vastavalt kas andmed on kirjutatud andmete või tehingu logifailidesse.
>
> Selleks, et õigesti geo juhendist andmed ja säilitada ühtse andmebaasi pilt, kõik üheksa VHDs sisu oleks geo kopeeritud/v-toimingud olid täidetud üheksas eri VHDs suhtes täpselt samas järjekorras. Azure Storage geo-dispersioonanalüüs ei võimalda sõltuvuste VHDs deklareerida. See tähendab, et Microsoft Azure'i Tabelimäluga geo-dispersioonanalüüs ei tea asjaolu, et need üheksa eri VHDs sisu on omavahel seotud ja andmete muudatused vastavad, ainult siis, kui imitatsiooniga järjestuses/v-toimingud, mis juhtus üle kõik 9 VHDs.
>
> Lisaks on kõrge, et seda stsenaariumi geo kopeeritud pilte ei paku ühtse andmebaasi pilt võimalusi, ka on jõudluse geo liigsete Storage, mis võib oluliselt mõjutada jõudlust kuvatakse. Kokkuvõttes kasutage seda tüüpi salvestusruumi koondamise DBMS tüüp töökoormus.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Vastendamise VHDs üheks Azure virtuaalse masina salvestusruumi kontod
Azure'i salvestusruumi konto on mõne haldus ehitada, vaid ka teema piirangud. Klõpsake kas räägitakse Azure'i Standard salvestusruumi konto või konto Azure Premium salvestusruumi erinevad piirangud. Täpse funktsioonid ja piirangud on loetletud [siin][storage-scalability-targets]
 
Nii, et Azure'i Standard Storage on oluline, on piirang IOPS salvestusruumi konto kohta (rea "Total taotlemine määra" sisaldavate [artiklist][storage-scalability-targets]). Lisaks on algse piirarvu 100 salvestusruumi kontode Azure tellimuse (juuli 2015) kohta. Seetõttu on soovitatav tasakaalustamiseks IOPS, VMs Azure'i Standard salvestusruumi kasutamisel mitme salvestusruumi kontode vahel. Samas ühele VM ideaalvariandis mitte kasutab ühe salvestusruumi võimalusel. Kui räägitakse DBMS juurutuste, kus iga VHD Azure'i Standard salvestusruumi majutatud võiks jõuda oma kvoodilimiit, peaks teil ainult juurutada 30-40 VHDs Azure'i salvestusruumi konto, mis kasutab Azure Standard salvestusruumi kohta. Teisalt, kui kasutada Azure Premium Storage ja soovite talletada suure andmebaasi maht, võib olla ilus nii IOPS. Kuid Azure Premium Storage konto on nii palju piiranguid andmete maht kui Azure Standard salvestusruumi konto. Selle tulemusena saate ainult juurutada teatud VHDs Azure Premium salvestusruumi kontol enne pihta andmete maht piiratud. Veebisaidil end mõelda konto Azure salvestusruumi nimega "Virtuaalse SAN", mis on piiratud võimalustele IOPS ja/või võimsus. Selle tulemusena tööülesanne jääb nagu asutusesiseste juurutuste määratleda erinevate SAP süsteemide VHDs paigutuse üle erinevate 'imaginary SAN seadmete' või Azure salvestusruumi kontod.
 
Azure'i Standard Storage pole soovitatav on salvestusruumi erinevate salvestusruumi kontodelt ühe VM võimalusel esitamiseks.

DS või GS-sarja Azure VMs abil on võimalik mount VHDs Azure Standard salvestusruumi kontod ja Premium salvestusruumi kontod. Kasutamise juhtudel nagu kirjutamine varukoopiate sisse kindlad tagatud VHDs pidades DBMS andmed ja logifailide Premium Storage tulevad meelde, kus heterogeensete säilitamine võidakse kasutada. 

Vastavalt kliendi juurutuste ja testimine umbes 30 40 saate VHDs andmebaasi andmefailid ja logifailid sisaldavad ette valmistatud ühe Azure'i Standard salvestusruumi kontol lubatud täitmist. Nagu varem mainitud, konto Azure Premium salvestusruumi piiramist on tõenäoliselt see mahub andmete võimsus ja mitte IOPS.

Kui SAN seadmete asutusesiseselt, ühiskasutuse nõuab jälgimise tuvastamaks lõpuks kitsaskohtade konto Azure salvestusruumi. Azure'i jälgimise laiend SAP-i ja Azure portaali on tööriistu, mida saab kasutada hõivatud Azure salvestusruumi kontod, mis võib olla pakkuda optimaalsetesse IO jõudluse tuvastamiseks.  Kui olukord on avastatud, on soovitatav teisaldada teise Azure'i salvestusruumi kontole hõivatud VMs. Vaadake [juurutamise juhend] [juurutusjuhend] täpsemat teavet selle SAP-i hosti jälgimise võimaluste aktiveerimiseks.

Kokkuvõttev sõnum heade tavade Azure'i Standard salvestusruumi ja Azure Standard salvestusruumi kontod ümber ka artiklist leiate siit <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Jooksev juurutatud DBMS VMs Azure'i Standard salvestusruumist Azure Premium Storage
Me ilmneda mõnda stsenaariumid, kus klient soovite liikuda juurutatud VM Azure Standard salvestusruumi Azure Premium Storage. See ei ole võimalik liikumata füüsilise andmed. On mitu võimalust eesmärgi saavutamiseks.

* Võib lihtsalt kopeerige kõik VHDs, base VHD kui ka andmete VHDs Azure Premium salvestusruumi uuele kontole. Sageli valisite VHDs arvu Azure Standard Storage pole seetõttu, et teil on vaja andmete maht. Kuid paljud VHDs vajalikuks selle IOPS. Nüüd, kui teisaldate Azure Premium Storage võib minna viis vähem VHDs mõned IOPS läbilaskevõime saavutamiseks. Arvestades, et Azure Standard Storage maksate kasutatud andmed ja pole nominal ketta mahtu, VHDs arv ei ole väga oluline kulude. Siiski Azure Premium Storage, mille soovite maksta nominal ketta mahtu. Seetõttu proovige kõige kliendi hoida Azure'i VHDs arvu Premium laos vajalikud saavutamiseks IOPS läbilaskevõime numbril. Nii, et enamik kliente otsustada 1:1 lihtne viis Kopeeri.
* Kui see pole veel kinnitatud, ühendate ühe VHD, mis sisaldab teie SAP-i andmebaasist varukoopia andmebaasi. Pärast varukoopia, lahutada kõik VHDs, sh VHD, mis sisaldab varundamine ja kopeerige base VHD ja selle VHD varukoopia Azure Premium Storage kontole. Saate siis VM põhjal base VHD juurutada ja kinnitage selle VHD koos varukoopia. Nüüd loote täiendavad tühja Premium salvestusruumi ketast VM kasutatavad üheks andmebaasi taastamine. See eeldab, et soovitud DBMS võimaldab teil muuta teed andmed ja log failide taastamine käigus.
* Teine võimalus on endise protsessi, kus lihtsalt kopeerige varundamise VHD Azure Premium Storage ja manustada selle vastu VM, mille äsja juurutatud ja installinud.
* Neljas võimalust, saate valida, kui teil on vaja andmeid oma andmebaasi failide arvu muutmine. Sellisel juhul tuleks teha ühtlast süsteemi koopia SAP-i abil impordi/ekspordi. Pane need failid eksportida VHD, mis kopeeritakse Azure Premium salvestusruumi kontole ja manustamiseks VM, mille abil saate käivitada impordi protsessid. Klientide peamiselt siis, kui kasutajad soovivad andmete failide arvu vähendamiseks seda võimalust kasutada.

### <a name="deployment-of-vms-for-sap-in-azure"></a>SAP-i Azure vms juurutamine
Microsoft Azure'i pakub mitmesuguseid võimalusi juurutada VMs ja seotud ketast. Seetõttu on väga oluline mõista nende erinevusi ettevalmistused VMs võib olla erinev sõltuvad juurutamise teel. Üldiselt vaatame stsenaariumid, mis on kirjeldatud järgmisi peatükke.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Azure'i turuplatsilt VM juurutamine
Soovite Microsoft või 3 tootja esitatud pilt Azure'i turuplatsilt juurutada oma VM. Kui olete juurutanud oma VM Azure, järgite samad juhised ja tööriistad installimiseks SAP-i tarkvara oma VM sees, nagu teeksite asutusesiseses keskkonnas. SAP-i tarkvara sees Azure VM installimist SAP-i ja Microsoft on soovitatav üles laadida ja talletada SAP-i installikandja Azure'i VHDs või luua mõne Azure VM töötamine "faili server", mis sisaldab kõiki vajalikke SAP-i installikandja.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Kliendi teatud generalized Image VM juurutamine
Tõttu teatud paikade seoses oma OS või DBMS versiooni, võib Azure'i turuplatsi esitatud pilte ei vasta teie vajadustele. Seetõttu võib juhtuda, kasutades oma "era" OS/DBMS VM pilt, mida saab kasutada hiljem korduvalt VM loomiseks. Kordamise "era" pildi ettevalmistamine, peab OS üldiste kohapealse VM. Vaadake [juurutamise juhend] [juurutusjuhend] täpsemat teavet üldistada VM.

Kui arvutisse on juba installitud SAP-i sisu oma kohapealse VM (eriti jaoks 2-astme süsteemid), saate kohandada SAP-i süsteemisätetest pärast Azure VM kaudu eksemplari juurutamise ümbernimetamine toiming ei toeta SAP-i tarkvara ettevalmistamise Manager (SAP-i Märkus [1619720]). Muul juhul saate installida SAP-i tarkvara hiljem pärast Azure VM kasutuselevõtt.
 
Andmebaasi sisu SAP-i rakendus kasutab, saate teha luua, sisu värskelt SAP-i installi või sisu saate importida Azure, kasutades on VHD DBMS andmebaasi varundamine või rakendusse Microsoft Azure'i Tabelimäluga DBMS otse varundada võimaluste kasutamine. Sel juhul võib ka ette valmistada VHDs DBMS andmed ja log failide asutusesisese ja seejärel importige need nimega ketast Azure. Kuid DBMS andmeid, mis laaditakse asutusesisesest Azure töötaks VHD ketast, mis peavad olema valmis kohapealse üle.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Jooksev VM kohapealse Azure-üldiste kettal
Plaanite teatud SAP-i süsteemis kohapealse liikumiseks Azure (lifti ja shift). Seda saab teha üleslaadimisel VHD, mis sisaldab Opsüsteemi, SAP-i kahendfaile ja lõpuks DBMS kahendfaile pluss VHDs DBMS Azure'i andmed ja logifailide failidega. Rakenduses vastas stsenaarium #2 ülaltoodud hoiate hostname, SAP-SID ja SAP-i kasutajakontode Azure VM vastavalt konfigureeritud asutusesiseses keskkonnas. Seetõttu üldistamist pilt ei ole vaja. Sel juhul rakendatakse enamasti asutusesiseses stsenaariumid, kus osa SAP-i maastiku käitatakse kohapealse ja osa Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Kõrge-saadavus ja avariitaastet Azure vms
Azure'i pakub järgmist kõrge kättesaadavus (HA) ja katastroofi taastamine (DR) funktsioone, mida me ei kasuta SAP-i ja DBMS juurutuste eri osade rakendamine

### <a name="vms-deployed-on-azure-nodes"></a>Azure sõlmed juurutatud VMs
Azure'i platvormi ei paku funktsioone nagu migreerimise juurutatud vms. See tähendab, et kui server kobar, millel on juurutatud VM on vajalikud hooldustööd, VM peab saavad peatada ja uuesti. Hoolduse Azure toimub nn täiendamine domeenide sees kogumite serverite abil. Ainult üks versiooniuuenduse domeeni korraga säilitatakse. Ajal selliste uuesti on teenuse katkemise ajal VM on välja lülitatud, hooldust tehakse ja VM uuesti. Enamik DBMS tarnijad pakuvad siiski kättesaadavuse ja avariitaastet funktsioone, mis kiiresti uuesti DBMS teenust teise sõlme kui esmane sõlm pole saadaval. Azure'i platvormi pakub funktsionaalsus levitada VMs, salvestusruumi ja muude Azure'i teenuste tagamaks, et kavandatud hooldustööd või taristu tõrgete mõjutaks ainult väikese osa VMs või teenuste domeenides täiendamine.  Ettevaatlik plaanimise on võimalik kohapealse infrastruktuuri kättesaadavus tasemele viia.

Microsoft Azure'i kättesaadavus terminikomplektid on loogiline rühmitamine vms või teenuseid, mis tagab VMs ja muude teenuste levitatakse eri viga ja täiendamine domeenide klaster sees nii, et oleks ainult üks sõlm sulgumist mis tahes ajal punktis (lugege [seda] [ virtual-machines-manage-availability] artikli üksikasjade).

See peab olema konfigureeritud otstarbe järgi, kui jooksvalt läbi VMs nagu siin näha:

![Definitsiooni DBMS HA konfiguratsioone kättesaadavus.][dbms-guide-figure-200]

Kui soovime väga kättesaadav konfiguratsioone DBMS juurutuste loomine (sõltumatu üksikud DBMS HA funktsioone kasutada), DBMS VMs oleks vaja:

* Lisage VMs sama Azure virtuaalse võrku (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* HA konfiguratsiooni VMs peaks olema sama alamvõrgu. Nimelahendus teise alamvõrku vahel pole võimalik vaid juurutuste, töötab ainult IP eraldusvõime. Rist-juurutustes-saidilt või ExpressRoute ühenduvuse kasutamise vähemalt üks alamvõrgu võrgu juba luuakse. Nimelahendus tehakse vastavalt asutusesisese AD poliitika ja võrgu taristu. 
[kommentaari]: <>  (MSSedusch TODO Test, kui tõene endiselt rühmas)

#### <a name="ip-addresses"></a>IP-aadressid
See on soovitatav häälestamine VMs jaoks HA olles viisil. IP-aadressid aadress HA partner(s) jooksul HA konfiguratsiooni tuginedes ei ole usaldusväärne Azure, kui kasutatakse staatiline IP-aadressid. Azure'i on kaks "Sulgumist" põhimõtet:

* Azure portaali või Azure PowerShelli cmdlet-käsk Peata-AzureRmVM kaudu sulgeda: sel juhul virtuaalse masina saab sulgemine ja tühistage eraldatud. Azure'i konto võetakse enam selle VM nii, et ainult kulud, mis tekivad on kasutatud hoidmiseks. Kui privaatne võrgu liidese IP-aadress ei staatilise, IP-aadress on välja ja see on tagatud, et võrgu liidese saab uuesti pärast uuesti VM määratud vana IP-aadressi. Läbimiseks siis alla Azure portaali kaudu või helistades Peata-AzureRmVM automaatselt põhjustada de eraldamine. Kui te ei soovi deallocat masina kasutamise lõpetamise-AzureRmVM - StayProvisioned 
* Mõne OS tasemel VM sulgemisel VM saab sulgeda ja tühistage eraldamata. Siiski sel juhul Azure'i konto endiselt tuleb tasuda VM, vaatamata sellele, et see on sulgumist. Sellisel juhul ülesande IP-aadressi peatatud VM jäävad samaks. Sulgemise VM kaudu sees on jõustada automaatselt de eraldamine.

Isegi jaoks asutusesiseses stsenaariumid, vaikimisi Sule ja de eraldamine tähendab de ülesande IP-aadresside VM, isegi juhul, kui kohapealse poliitikate DHCP sätted on erinevad. 

* Erandiks on, kui üks määrab staatiline IP-aadress võrgu liidese nimega kirjeldatud [siin][virtual-networks-reserved-private-ip].
* Sellisel juhul IP-aadress on fikseeritud niikaua, kuni võrgu liidese ei kustutata.

> [AZURE.IMPORTANT] Selleks, et hoida kogu juurutamise lihtsaid ja mõistliku, Eemalda soovitus on häälestus on DBMS HA või DR konfiguratsiooni Azure'is viisil, mis on seotud erinevate VMs toimiva nimelahendus partneriks VMs.
 
## <a name="deployment-of-host-monitoring"></a>Juurutamise hosti jälgimine
SAP-i rakenduste Azure'i Virtuaalmasinates viljakamalt kasutamist, SAP-i nõuab võimalus host jälgida andmeid töötab Azure'i Virtuaalmasinates füüsilise hosts kuvada. SAP-i HostAgent paik taset on vaja seda võimalust SAPOSCOL ja SAP-i HostAgent, mis võimaldab. SAP-i Märkus [1409604]on dokumenteerida täpne paik tase.

Komponendid, mis annavad host andmete SAPOSCOL ja SAPHostAgent kasutuselevõtu ja elutsükli haldus nende komponentide kohta lisateabe saamiseks vaadake [juurutamise juhend] [juurutusjuhend]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Microsoft SQL serveri üksikasjad

### <a name="sql-server-iaas"></a>SQL Server IaaS
Microsoft Azure'i alustades, saate hõlpsasti migreerida olemasoleva SQL serveri rakenduste ehitatud platvorm Windows Server Azure'i Virtuaalmasinates. SQL Server virtuaalse masina võimaldab vähendada kogumaksumuse omandiõiguse juurutus, haldamine ja hooldus enterprise laius rakenduste kergesti migreerimine need rakendused Microsoft Azure. SQL serveriga on Azure virtuaalse masina, administraatorid ja arendajate saate siiski kasutada sama arendamise ja haldamise tööriistad, mis on saadaval kohapealse. 

> [AZURE.IMPORTANT] Pange tähele, et me ei käsitleta Microsoft Azure'i SQL-andmebaasi, mis on platvorm, nagu teenuse pakkumine Microsoft Azure'i platvormi. Arutelu selles dokumendis on toote SQL Server töötab, kui see on teada kohapealse juurutuse rakenduses Azure'i Virtual masinad, nagu teenuse võimalus Azure infrastruktuuri kasutamine. Andmebaasi võimaluste ja nende kahe pakkumiste vahel funktsioonid on erinevad ja ei tohi segada üksteisega. Vt ka: <https://azure.microsoft.com/services/sql-database/>
 
On soovitatav üle vaadata [selle] [ virtual-machines-sql-server-infrastructure-services] dokumentatsiooni enne jätkamist.

Järgnevalt on kokku liita ja mainitud tekstilõigule dokumentide linki jaotises osad. SAP-i ümber üksikasjad nimetatakse ka ja mõned mõisted on üksikasjalikult kirjeldatud. Aga see on soovitatav läbige dokumentatsiooni kohal esimene enne lugemist SQL serveri teatud dokumendid.

IaaS konkreetset teavet, mida peaksite teadma enne jätkamist mõnda SQL serveri on:

* **Virtuaalse masina SLA**: on SLA Virtuaalmasinates töötab Azure, mille saate siit: <https://azure.microsoft.com/support/legal/sla/>  
* **SQL-i versioon toetab**: SAP-i kliendid, toetame SQL Server 2008 R2 ja kõrgemale klõpsake Microsoft Azure virtuaalse masina. Varasemad ei toetata. Vaadake üle üldine [Tugi lause](https://support.microsoft.com/kb/956893) rohkem üksikasju. Pange tähele, et üldiselt SQL Server 2008 Microsoft toetab samuti. Kuid tõttu märkimisväärset funktsionaalsuse SAP, mis võeti kasutusele SQL Server 2008 R2, SQL Server 2008 R2 on minimaalne väljaanne SAP-i jaoks. Pidage meeles, kas teil on SQL Server 2012 – 2014 laiendatud süvitsi integreerimise IaaS stsenaariumi (nt varundamine otse vastu Azure Storage). Seetõttu me piirata st SQL Server 2012 – 2014 selle Viimane paik taseme Azure.
* **SQL-i funktsioonide toe**: kõige SQL serveri funktsioonid on toetatud Microsoft Azure'i Virtuaalmasinates on mõned erandid. **SQL serveri tõrkesiirdeklastrite kaudu ühiskasutusse antud ketast pole toetatud**.  Jaotatud tehnoloogiad, nagu andmebaasi peegeldus, Kättesaadavusrühmad, kopeerimine, Logi saatmine ja teenuse ta on toetatud ühtse Azure'i piirkonna. SQL serveri AlwaysOn on toetatud ka Azure erinevate piirkondade vahel nagu siin dokumenteerida: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Vaadake lisateavet [Tugiteenuste lause](https://support.microsoft.com/kb/956893) . Näide, juurutamise AlwaysOn konfigureerimisel on näidatud [selles] [ virtual-machines-workload-template-sql-alwayson] artiklis. Samuti kontrollida selle head tavad dokumenteeritud [siin][virtual-machines-sql-server-infrastructure-services] 
* **SQL-i jõudlus**: Oleme kindel, et Microsoft Azure'i Virtuaalmasinates majutatud täidab hästi võrreldes teiste avaliku pilve virtualization pakkumisi, kuid üksikute tulemused võivad erineda. Vaadake [seda] [ virtual-machines-sql-server-performance-best-practices] artikkel.
* **Azure'i turuplatsilt piltide abil**: kiireim viis juurutamiseks uue Microsoft Azure VM on pildi kasutamiseks Azure'i turuplatsilt. Azure'i turuplatsil pilte, mis sisaldavad SQL Server on. Pildid, kus on SQL Server juba installitud rakenduste SAP NetWeaver ei saa kohe kasutada. Põhjus, miks on need pildid ja mitte kogumit, mis on nõutud SAP NetWeaver süsteemid on installitud vaikimisi SQL serveri võrdlemine. Selleks, et kasutada selliseid pilte, kontrollige juhiseid dokumenteerida Peatükk [abil SQL serveri pildid välja Microsoft Azure'i turuplatsi] [dbms-juhend-5.6]. 
* Vaadake lisateavet [Hinnad üksikasjad](https://azure.microsoft.com/pricing/) . Ka [SQL Server 2012 hulgilitsentsimise juhend](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) ja [SQL serveri 2014 hulgilitsentsimise juhend](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) on oluline ressurss.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>SQL serveri konfigureerimise juhised SAP-i seotud Azure VM SQL serveri installid

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>SAP-i VM/VHD struktuuri soovitused seotud SQL serveri juurutuste
Üldise kirjelduse, tuleks SQL serveri täitmisfailid asub või installitud süsteemi draivi soovitud VM base VHD (drive C:\).  Tavaliselt enamik SQL serveri andmebaaside süsteemi ei kasutata kõrge, SAP NetWeaver töökoormus. Seega saate süsteemi andmebaasid SQL serveri (juhtslaidi, msdb ja mudeli) jäävad selle C:\ kettadraivi. Erandi võib olla tempdb, mis mõned SAP ERP ja kõik BW töökoormus, puhul võidakse nõuda suurem andmete maht või I/O toimingute maht, mis ei sobi algse VM. Selliste süsteemide jaoks tuleb teha järgmist:

* Peamine andmete failid SAP-i andmebaasi loogilise samale kettale esmane tempdb andmefailide liikuda.
* Täiendavad tempdb andmete faile lisada iga muude andmefaili SAP-i kasutajate andmebaasi, mis sisaldab loogilised draivid.
* Loogiline ketas, mis sisaldab kasutajate andmebaasi logifaili tempdb logifaili lisada.
* **Ainult VM jaoks, mis kasutavad kohaliku SSD** Arvuta sõlm tempdb andmete ja logide failid võivad paigutada D:\ ketas. Siiski võib soovitatav kasutada mitut tempdb andmefaile. Arvestage D:\ ketas maht on erinevad sõltuvalt VM.
 
Need võimaldavad tempdb rohkem ruumi, kui draivi, kuhu on võimalik anda. Selleks, et määrata õige tempdb suurus, saate kontrollida, tempdb suurused käivitada kohapealse olemasoleva arvutites. Lisaks võimaldaks sellise konfiguratsiooni tempdb, mida ei saa anda draivi IOPS arvud. Klõpsake uuesti süsteemides, kus töötab kohapealse saab kasutada nii, et võite saada IOPS arvude suhtes tempdb I/O töökoormus jälgimiseks ootate oma tempdb näidata.

Näeks VM konfiguratsiooni, mis töötab SQL Serveri andmebaas SAP-i ja kus tempdb andmed ja tempdb logifaili paigutatakse D:\ ketas:
 
![Azure'i IaaS VM SAP viide konfigureerimine][dbms-guide-figure-300]

Pange tähele, et D:\ draivil erineva suuruse järgnevaid VM tippige. Nõue tempdb suurus sõltub teie võib kasutajanimeks sidumiseks tempdb andmete ja logide failid SAP-i andmebaasi andmete ja logide kui D:\ ketas on liiga väike.

#### <a name="formatting-the-vhds"></a>Vorming soovitud VHDs
SQL serveri NTFS-i blokeerida suurus VHDs SQL serveri andmeid sisaldav ja logifailide peaks olema 64K. Ei ole vaja D:\ draivi vormindamiseks. See ketas sisaldab eelvormindatud.

Selleks, et veenduda, et taastada või andmebaaside loomine ei käivitub andmefailid, nullidega failide sisu, ühte veenduge, et kasutaja kontekstis, mis töötab SQL Serveri teenuse on teatud luba. Windowsi administraator rühma kasutajad on tavaliselt järgmisi õigusi. Kui SQL Serveri teenuse käitatakse Windows administraator kasutaja kontekstis kasutaja, peate määrata selle kasutaja kasutaja paremale "Mahu haldamise toimingud".  Vt Microsofti teabebaasi artiklit üksikasjad: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Andmebaasi tihendamise mõju
Konfiguratsiooni, kus I/O läbilaskevõime võib muutuda piiramine tegurit, võivad aidata iga mõõt, mis vähendab IOPS venitada ühte käitamise IaaS stsenaariumis nagu Azure'i töökoormus. Seetõttu, kui see pole veel valmis, rakendades SQL Server PAGE tihendamise soovitame Microsofti ja SAP-i enne üles mõni olemasolev SAP-i andmebaasid Azure.
 
Soovitus teha enne üleslaadimine Azure'i andmebaasi tihendamise antakse välja kahel põhjusel:

* Andmete üleslaadimist on väiksem.
* Tihendamise täitmise kestus on lühem, eeldades, et tugevam riistvara saab kasutada rohkem protsessorid või uuem versioon I/O läbilaskevõime või vähem I/O latentsus kohapealse.
* Andmebaasi väiksemad võib kaasa tuua vähem kettaruumi eraldatud kulud

Andmebaasi tihendamise toimib samuti sisse ka Azure'i Virtuaalmasinates kohapealse tagamisse. Üksikasjalikumat teavet tihendamise SAP-i olemasoleva SQL serveri andmebaasi Vaadake siin: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>SQL serveri 2014 – andmebaasi failide salvestamine otse Azure'i ajaveebi säilitamise kohta
SQL serveri 2014 avaneb võimalus otse poes Azure'i bloobimälu ilma nende ümber on VHD ümbris andmebaasifaile talletada. Eriti Standard Azure Storage või väiksema VM tüüpi abil see võimaldab stsenaariumid, kus saate lahendada IOPS, mida soovite täita teatud VHDs, mida saab kinnitada mõne väiksema VM tüüpi piirid. See toimib kasutaja andmebaaside, kuid mitte süsteemi andmebaaside SQL serveri jaoks. See töötab ka SQL serveri andmeid ja log failide jaoks. Kui soovite juurutada SAP-i SQL serveri andmebaasi asemel "mähkimine" sellisel viisil see VHDs, pidage järgmist meeles:

* Salvestusruumi konto varem vajadustele Azure'i piirkonna nagu ühte, mida kasutatakse VM SQL serveri juurutamine töötab.
* Loetletud seoses levitamine VHDs üle erinevate Azure salvestusruumi kontod asjaoluga rakendada ka juurutuste puhul. Tähendab, et selle piiri Azure Storage konto I/O toimingute arvu.
[kommentaari]: <>  (MSSedusch TODO, kuid seda kasutatakse võrgu ribalaiust ja salvestusruumi ribalaiust, pole?)

Seda tüüpi juurutamise üksikasjad on loetletud siin: <https://msdn.microsoft.com/library/dn385720.aspx>
 
SQL serveri andmefailid otse Azure Premium Storage talletada, et peab teil olema vähemalt SQL Server 2014 paik väljaanne, mis on siin: <https://support.microsoft.com/kb/3063054>. SQL serveri andmefailid Azure Standard Storage talletamise töötada avaldatud versiooni SQL Server 2014. Siiski sama plaastrid sisaldavad parandusi, mis muudavad Azure'i bloobimälu otsest kasutamist SQL serveri andmefailid ja varukoopiate usaldusväärne teise rea. Seetõttu soovitame üldiselt need plaastrid kasutama.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL serveri 2014 puhvri Pool pikendamise
SQL serveri 2014 kasutusele uus funktsioon, mida nimetatakse puhvri Pool pikendamise. See funktsioon laieneb puhvri pargi SQL serveri, mis on teise taseme vahemälu, mis toetavad kohaliku SSD serveri või VM mälu säilitada. See võimaldab hoida suuremat töökomplekt andmete "mälu". Juurdepääs Azure'i Standard salvestusruumi võrreldes Accessi rakendusse puhvri puhvri, mis on talletatud kohalik SSD on Azure VM laiend on palju tegureid kiiremini.  Seetõttu kasutamine on suurepärane IOPS ja läbilaskevõime VM tüüpi D:\ kõvakettale võib olla väga mõistlik võimalus vähendada IOPS Azure Storage vastu ja parandada vastuse korda päringute oluliselt. See kehtib eriti siis, kui ei kasuta Premium mälu. Premium salvestusruumi ja Premium Azure'i lugege teemat vahemälu Arvuta sõlme kasutamist korral soovitatava andmefailid, suur erinevusi oodatakse. Põhjus on selles, et nii vahemälu (SQL serveri puhvri Pool pikendamise ja Premium salvestusruumi lugege teemat vahemälu) kasutate kohaliku ketast Arvuta sõlmed.
Selle funktsiooni kohta leiate Lisateavet vaadake seda dokumentatsiooni: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>SQL serveri varundamise ja taastamise peaksite arvesse võtma
SQL Server Azure'i juurutamisel oma varukoopia meetodi läbi vaadata. Isegi juhul, kui süsteemi ei ole produktiivne süsteemi, SAP-i korraldaja SQL serveri andmebaasi tuleb varundada perioodiliselt. Azure Storage hoiab kolm pilti, seetõttu on nüüd varukoopia väiksemaid osas kompensatsiooni salvestusruumi krahh. Priority (prioriteet) põhjus säilitades proper varundamine ja taastamine leping on rohkem, et saate esitada punkti kellaaja funktsioonid loogiline/käsitsi tõrkeid kompenseeri. Nii, et eesmärk on kas kasutamine varukoopiate taastada andmebaas on teatud aja või kasutada varukoopiaid Azure seeme teise süsteemi olemasoleva andmebaasi kopeerimise abil. Näiteks võib kannate 2-astme SAP-i konfigureerimine 3 süsteemi häälestus sama süsteemi järgi taastamine varukoopia.

On kolm võimalust varukoopia SQL Server Azure'i salvestusruumi.
 
1. SQL Server 2012 CU4 ja kõrgema saab algupäraselt varukoopia andmebaase URL-i. See on üksikasjalikult kirjeldatud ajaveebi [uued funktsioonid rakenduses SQL Server 2014 – osa 5 – varundus ja taaste täiustused](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Vt peatükk [SQL Server 2012 SP1 CU4 ja uuemad versioonid] [dbms-juhend-5.5.1].
1. SQL serveri väljalasete enne SQL 2012 CU4 saate ümbersuunamise funktsioonid on VHD varundamine ja voo kirjutamine põhiliselt liikuda Azure Storage asukohta, mis on konfigureeritud. Vt peatükk [SQL Server 2012 SP1 CU3 ja varasemates versioonides] [dbms-juhend-5.5.2].
1. Lõplik meetod on tavapärase SQL serveri varundamine ketta käsk VHD ketas seadmesse.  See on identne kohapealse juurutuse mustri ja käsitletakse ei üksikasjalikult selles dokumendis.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 ja uuemad versioonid
Selle funktsiooni abil saate otse Azure'i bloobimälu varukoopia. Ilma seda meetodit saate varundada muude Azure'i VHDs, mida soovite kasutada VHD ja IOPS abil. Idee on põhiliselt järgmine:
 
 ![Varundamise SQL Server 2012 mäluruumi Microsoft Azure'i BLOOBIMÄLU][dbms-guide-figure-400]

Sel juhul on see eelis, et üks ei pea veedavad VHDs SQL serveri varukoopiate salvestamiseks. Nii teil on vähem VHDs eraldatud ja kogu ribalaiust VHD IOPS saab kasutada andmete ja logifailid. Varukoopia maksimaalne maht on kuni 1 TB nagu dokumenteerida selle artikli jaotist "Piirangud": <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Kui varukoopia suurus, vaatamata kasutades SQL serveri varukoopia tihendamise ületaks 1 TB suurus, funktsioonid, mis on kirjeldatud [SQL Server 2012 SP1 CU3 ja varasemates versioonides] [dbms-juhend-5.5.2] See dokument tuleb kasutada.

[Seotud dokumendid](https://msdn.microsoft.com/library/dn449492.aspx) , mis kirjeldab andmebaaside suhtes Azure'i bloobimälu poe varukoopiate põhjal taastamine ei soovita otse Azure'i BLOOBIMÄLU poest taastada, kui varukoopiaid on > 25 GB. Selle artikli soovitus põhineb lihtsalt jõudluse huvides ja ei tööta enam piirangutest. Seetõttu kehtivad erinevad tingimused juhtumi alusel.

[Selle](https://msdn.microsoft.com/library/dn466438.aspx) õpetuse leiate dokumentatsiooni kohta, kuidas seda tüüpi varundamise häälestada ja kasutada
 
Näide etappide jada saab lugeda [siin](https://msdn.microsoft.com/library/dn435916.aspx).

Toimingute automatiseerimine varukoopiate, on kõige olulisemad veendumaks, et iga varundamiseks plekid nimeks erinevalt. Kirjutatakse need muidu ja taaste ahelas on katkenud.
 
Selleks et mitte segada toiminguid 3 erinevat tüüpi varukoopiate vahel, on soovitatav eri ümbriste all kasutatav varukoopiate salvestusruumi konto loomiseks. Funktsiooni ümbriste võiks olla VM ainult või VM ja varundamise tüübi järgi. Skeemi näeb:
 
 ![SQL Server 2012 varundus Microsoft Azure'i salvestusruumi BLOOBIMÄLU – eri ümbriste jaotise eraldi salvestusruumi konto abil][dbms-guide-figure-500]

Ülaltoodud näites oleks ei tehta varukoopiaid, salvestusruumi samale kontole, kus on juurutatud VMs. Uue konto salvestusruumi spetsiaalselt varufailide oleks. Salvestusruumi kontod ning sees oleks eri tüüpi varundus ja VM nime maatriksi loodud ümbriste. Sellise osadeks lihtsustab võimaluse haldamiseks seonduvale eri VMs varukoopiaid.

Plekid üks otse kirjutab varukoopiate abil, pole arvu VHDs VM, lisades. Seega ühte võib maksimeerimine VHDs paigaldatud, teatud VM SKU andmete jaoks kõige rohkem ja tehingu logifail ja varukoopia salvestusruumi container suhtes siiski käivitada. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 ja varasemates versioonides
Kõigepealt peate tegema varukoopia otse vastu Azure Storage saavutamiseks oleks msi, mis on seotud [artiklit KBA](https://www.microsoft.com/download/details.aspx?id=40740) alla laadida.
 
Laadi alla x64 installifail ja dokumentatsiooni kohta. Faili installib programmi nimega: "Microsoft SQL serveri varukoopia Microsoft Azure'i tööriista". Lugege toote dokumentatsiooni hoolikalt.  Tööriist töötab põhiliselt järgmisel viisil:

* SQL serveri servast, SQL serveri varukoopia ketta asukoht on määratletud (Ärge kasutage D:\ ketas nii).
* Tööriist võimaldab teil määrata reeglid, mida saab kasutada eri tüüpi varukoopiaid erinevate Azure Storage ümbriste suunamiseks.
* Kui reegleid on kohas, suunab tööriista varukoopia voo kirjutamine ühele VHDs/ketast Azure Storage asukohta, mis on määratletud varasemas versioonis.
* Tööriist jätab mõne KB suurust faili väike konts VHD/kettale, mis on määratletud SQL serveri varukoopia. **Selle faili tuleks talletuskoht jätta, kuna see on vajalik Azure Storage uuesti taastamine.**
    * Kui teil on kadunud konts faili (nt kaudu kaotsimineku elektrooniliselt, kus asunud konts fail) ja olete valinud suvandi Microsoft Azure Storage konto varundada, saab taastada konts faili läbi Microsoft Azure'i Tabelimäluga see oli paigutatud salvestusruumi ümbris alla laadida. Seejärel tuleks asetada konts fail kohalikus arvutis kaust, kuhu tööriist on konfigureeritud tuvastada ja üles laadida samal pakendi sama krüptimine parooliga krüptimise kasutati algse reegliga. 

See tähendab, et skeemiga eespool kirjeldatud uuemates versioonides SQL serveri jaoks saab luua ka SQL serveri väljalasete, mis on jäetud aadress on Azure talletuskoht alles.
 
See meetod ei tohi kasutada koos uuemasse SQL serveri keskkonda, mis toetavad varundamise üles algupäraselt Azure Storage suhtes. Erandiks on, kus piirangud kohalikke varukoopia Azure'i sisse blokeerivad kohalikke varukoopia täitmise Azure'i sisse.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Muud võimalused varukoopia SQL Serveri andmebaasiga
Muud võimalused varukoopia andmebaasidele on täiendavad VHDs manustamiseks VM, mida kasutate varukoopiate salvestamiseks. Sellisel juhul peaksite veenduge, et selle VHDs ei tööta täielik. Kui see on nii, peaksite lahutada on VHD ja nii rääkida "arhiivimine" ja asendage see uue tühja VHD. Kui avate seda teed, mida soovite säilitada need VHDs eraldi Azure salvestusruumi kontod seast mis VHDs andmebaasi faile.

Teine võimalus on suur VM, mis võib olla manustatud palju VHDs kasutada. Nt D14 koos 32VHDs. Kasutage salvestusruum paindlik keskkonnas kui koostate võib ühtlasi, mida kasutatakse siis varukoopia sihtkohtade erinevate DBMS serverite jaoks.
 
Kas teil on dokumenteerida parimaid tavasid [siin](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) ka. 

#### <a name="performance-considerations-for-backupsrestores"></a>Varukoopiate/taastab kohta
Nagu tühjal metallist juurutuste varundus ja taaste jõudlus sõltub mitu mahud saab lugeda paralleelselt ja nende mahud läbilaskevõime võib olla. Lisaks kasutavad varukoopia tihendamise CPU tarbimine võib mängida olulised VMs koos ainult kuni 8 CPU teemad. Seetõttu üks täita:

* Vähem soovitud arvu VHDs kasutatud andmete talletamiseks faili, mida väiksem üldine jõudlus lugemisel.
* Mida väiksem arv Teemad VM rohkem raske varukoopia tihendamise mõju.
* Vähem eesmärkide (plekid või VHDs) kirjutamise varukoopia, olenevalt sellest, kumb on läbilaskevõime.
* Mida väiksem VM suuruse, väiksemate läbilaskevõime salvestuslimiidi kirjutamine ja Azure Storage lugemine. Kas varukoopiaid otse talletatud Azure'i bloobimälu, või kas need on talletatud VHDs, mis on talletatud uuesti Azure plekid sõltumata.

Microsoft Azure'i salvestusruumi BLOOBIMÄLU varukoopia eesmärgi uuemate versioonidega kasutamisel olete piiratud määrata ainult üks URL-i siht iga teatud varukoopia.
 
Kuid vanemate versioonidega "Microsoft SQL serveri varukoopia Microsoft Azure'i tööriista" kasutamisel saate määratleda rohkem kui üks fail eesmärk. Rohkem kui üks eesmärk, saate mastaapimiseks varundamine ja läbilaskevõime varukoopia on suurem. See siis tulemuseks mitu faili ka Azure Storage konto. Meie katsetamine, kasutades ühte kindlasti võite saavutada läbilaskevõime, kus üks saavutada varukoopia laiendiga mitu faili sihtkohta rakendada: SQL Server 2012 SP1 CU4 kohta. Saate ka mitte blokeeritud 1TB limiit nagu kohalikke varundamise Azure'i sisse.

Siiski pidage meeles, on läbilaskevõime ka sõltub asukoha Azure Storage konto kasutamiseks varukoopia. Idee võib olla leidmiseks salvestusruumi konto teises regioonis kui VMs töötab. Nt Saate käivitada VM konfiguratsiooni Lääne-Euroopa, kuid sellele salvestusruumi kontot, mille abil saate uuesti kuni vastu Põhja-Euroopa. Mis on varukoopia läbilaskevõime mõju kindlasti ja tõenäoliselt luua läbilaskevõime 150MB/SEK, kui see on võimalik, juhul kui target salvestusruumi ja VMs töötavad sama piirkondliku andmekeskuse.

#### <a name="managing-backup-blobs"></a>Varukoopia plekid haldamine
On nõue varukoopiate ise hallata. Kuna eeldatakse palju plekid luuakse täites sagedased tehingu varundamine, nende plekid manustamine hõlpsalt saab koormata Azure'i portaal. Seetõttu on soovitav kasutada Azure salvestusruumi Explorer. On saadaval mitu hea need, mis aitab Azure storage konto haldamine

* Microsoft Visual Studio Azure'i SDK installitud (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure'i salvestusruumi Explorer (<https://azure.microsoft.com/downloads/>)
* 3. osapoole tööriistu

[kommentaari]: <>  (Arm pole veel toetatud)
[kommentaari]: <>  (## Azure VM varundamise)
[kommentaari]: <> SAP-i süsteemis  (VMs saab varundada Azure virtuaalse masina varundamise funktsiooni. Azure virtuaalse masina varukoopia sain sisse alguses, 2015 ja samal ajal on Standardne meetod, et varundamine on lõpule viidud VM Azure. Azure'i varundamine varukoopiaid talletab Azure ja võimaldab taastamine VM uuesti.) 
[kommentaari]: <> (VM, mis töötavad andmebaaside saab varundada sobival viisil samuti kui DBMS süsteemide toetab Windows VSS (draivi vari teenus - <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>), nagu näiteks SQL Server ei. Nii, et Azure VM varundamise võib olla võimalus leida tagastatav SAP-i andmebaasi varukoopia. Siiski tuleb see punkti õigel ajal taastab andmebaaside Azure VM varukoopiate põhjal ei ole võimalik. Seega soovitus on teha andmebaaside DBMS funktsioonidega selle asemel Azure VM varukoopia.) [kommentaari]: <> (saate tutvuda Azure virtuaalse masina varukoopia palun alustage siin <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>SQL serveri piltide abil välja Microsoft Azure'i turuplatsilt
Pakub Microsoft Azure'i turuplatsi VMs, mis juba sisaldab SQL serveri versioon. SAP-i klientidele, kes on vaja litsentse Windows ja SQL serveri, see võib olla võimalus põhiliselt kataks vaja litsentside valmistamata üles VMs SQL Server juba installitud. SAP-i piltide kasutamiseks on vaja teha järgmisega:

* SQL serveri-hindamine versiooni hankimine suuremad kulud lihtsalt 'Ainult Windows' VM juurutatud Azure'i turuplatsilt. Lugege järgmisi artikleid võrdlemiseks hinnad: <https://azure.microsoft.com/pricing/details/virtual-machines/> ja <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* Saate kasutada ainult SQL serveri väljalasete, mis ei toeta SAP-i, nt SQL Server 2012.
* SQL serveri eksemplar, kuhu on installitud pakutud Azure'i turuplatsi VMs võrdlemine pole võrdlemine, SAP NetWeaver nõuab SQL serveri eksemplar käivitamiseks. Saate muuta selle võrdlemine kuigi juhistega kohta järgmisest jaotisest.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>SQL serveri võrdlemine Microsoft Windowsi/SQL serveri VM muutmine
Kuna pildid SQL Server Azure'i turuplatsil võrdlemine, mida on vaja SAP NetWeaver rakenduste kasutamiseks on häälestatud, tuleb kohe pärast juurutamise muuta. SQL Server 2012, seda saab teha järgmiste toimingute abil niipea, kui VM kasutusele võetud ja administraator saab sisse logida juurutatud VM:

* Avage käsuviiba aken Windowsi "administraatorina".
* Saate muuta kataloogi C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Käsu: Setup.exe /QUIET või meetme kavandatavatest = REBUILDDATABASE /INSTANCENAME = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> on konto, mis on määratletud administraatorikonto VM juurutamisel Galerii esimest korda.

Protsess peaks ainult kuluda mitu minutit. Selleks, et veenduda õige tulemuse lõppes etappi, tehke järgmist.

* Avatud SQL Server Management Studio.
* Päringu akna avamine
* SQL Serveri andmebaasiga juhtslaidi käsu sp_helpsort käivitada.

Soovitud tulemuse peaks välja nägema.

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Kui tulem peatada juurutamine SAP-i ja uurida, miks käsk häälestus ei tööta ootuspäraselt. SAP NetWeaver rakenduste peale SQL serveri eksemplar ja muu SQL serveri codepages kui üks mainitud kohal **ei** toetata.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SAP Azure SQL serveri kõrge-saadavus
Nagu varem st, ei ole võimalik luua ühiskasutusega salvestusruumi, mis on vajalik vanimast SQL serveri kõrge-saadavus funktsioonide kasutamist. See funktsioon oleks installimine kahe või enama SQL Serveri eksemplari rakenduses on Windows Server Tõrkesiirde kobar (WSFC) ühiskasutusega kettale kasutamise kasutaja andmebaasid (ja lõpuks tempdb). See on palju aega standard kõrge-saadavus meetod, mida toetavad ka SAP-i. SQL serveri kõrge-saadavus konfiguratsioone ühiskasutusega kettale kobar konfiguratsiooni ei saa aru, kuna Azure'i ei toeta ühiskasutusse antud salvestusruumi. Siiski palju kõrge-saadavus meetodite on võimalik ja on kirjeldatud järgmistes lõikudes.

[kommentaari]: <>  (Artiklis on endiselt viidates ASM)
[kommentaari]: <>  (Enne lugemist erinevate teatud kõrge-saadavus tehnoloogiad kasutatav SQL Server Azure'i, on hea dokumendi, mis annab rohkem üksikasju ja viitu [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions])

#### <a name="sql-server-log-shipping"></a>SQL serveri Logi saatmine
Kõrge kättesaadavus (HA) meetodite on SQL serveri logifailide saatmine. Kui osalemine HA konfiguratsiooni VMs on töötamise nimelahendus, pole probleemi ja Azure häälestamise ei erine mis tahes häälestamine on tehtud kohapealse. Ei ole soovitatav kasutada ainult IP eraldusvõime. Seoses häälestamise Log saatmis- ja põhimõtted ümber Logi saatmine Kontrollige dokumentide:

<https://technet.microsoft.com/library/ms187103.aspx>
 
Mis tahes kõrge kättesaadavus väga saavutamiseks tuleb juurutada VMs, mis kuuluvad vastava Logi saatmine konfiguratsiooni jooksul sama Azure'i kättesaadavus seadmine.

#### <a name="database-mirroring"></a>Andmebaasi peegeldus
Andmebaasi peegeldamine nimega toeta SAP-i (vt märkus SAP-i [965908]) tugineb määratlemine Tõrkesiirde partneri SAP-i ühendusstring. Asutusesiseses juhtudel me endale kaks VMs on sama domeeni ja, et kasutaja konteksti kaks SQL Serveri eksemplari töötavad all on samuti domeeni kasutajad ja on kaasatud SQL serveri kahel piisavaid õigusi. Seetõttu ei erine seadistuse andmebaasi peegeldus Azure tüüpilised kohapealse häälestamise ja konfigureerimise.

Lihtsaim viis on vaid juurutuste on üks domeenis mõne muu domeeni häälestuses Azure'is, on need DBMS VMs (ja SAP-i ideaalvariandis mitte sihtotstarbeline VMs).

Kui domeeni ei ole võimalik, üks kasutada ka serdid andmebaasi peegeldus lõpp-punktid, nagu on kirjeldatud siin: <https://technet.microsoft.com/library/ms191477.aspx>

Õpetuse, et ülesseadmine andmebaasi peegeldus Azure leiate siit: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>AlwaysOn
Kuna AlwaysOn on toetatud jaoks SAP kohapealse (vt märkus SAP-i [1772688]), see on toetatud saab kasutada koos SAP-i Azure. Asjaolu, et te ei saa ühiskasutusega ketta loomiseks Azure ei tähenda, et üks ei saa luua eri VMs vahel konfigureerimisel AlwaysOn Windows Server Tõrkesiirde kobar (WSFC). Ainult tähendab see, et teil on võimalus kasutada ühiskasutusega kettale otsustusvõimeline kobar konfiguratsiooni. Seega saate koostada Azure konfigureerimisel AlwaysOn WSFC ja lihtsalt valige kvoorumi tüüp, mis kasutab ühiskasutusega kettale. Azure'i keskkonnas need on juurutatud lahenda VMs nime järgi ja VMs tuleks sama domeeni. See kehtib ainult Azure ja asukohaga juurutuste. On mõned ümber juurutamine SQL serveri kättesaadavus rühma kuulajale (mitte segi Azure'i kättesaadavus seadmine) silmas pidada Kuna Azure'i sel hetkel ei luba objekti AD/DNS-i lihtsalt loomiseks, et see on võimalik kohapealse. Seetõttu on vajalik teatud toimimist Azure'i mõne muu installimisjuhiseid.

On mõned kaalutlused on kättesaadavus rühma kuulajale abil.

* Mõne rühma kättesaadavus kuulajale abil on võimalik ainult Windows Server 2012 või Windows Server 2012 R2 VM külalisena OS. Windows Server 2012 peate veenduge, et see paik on rakendatud: <https://support.microsoft.com/kb/2854082> 
* Windows Server 2008 R2 pole see paik ja AlwaysOn oleks vaja kasutada samal viisil nagu andmebaasi peegeldus, määrates Tõrkesiirde partneri ühendused stringi (SAP-i default.pfl parameetri dbs/mss/serveri kaudu – valmis vt märkus SAP-i [965908]).
* Kui kasutate mõnda kättesaadavus rühma kuulajale, peate andmebaasi VMs sihtotstarbeline laadi koormusetasakaalustusteenuse olema ühendatud. Nimelahendus vaid kasutuselevõttu teha nõuaks, kõik VMs SAP-i süsteemi (rakenduste serverid, DBMS server ja (A) SCS server) on sama virtuaalse võrgu või nõuaks kaudu on SAP-i kihid etc\host faili säilitamine selleks, et saada lahendatud SQL serveri VMs VM nimed. Vältimiseks, et Azure'i on määramine uue IP-aadressid, kui mõlemad VMs on muidu sulgumist, üks peaks määrata staatiline IP-aadresside võrgu liidesed nende VMs AlwaysOn konfiguratsiooni (määratlemine staatiline IP-aadress on kirjeldatud [seda] [ virtual-networks-reserved-private-ip] artiklis) [kommentaari]: <> (vana ajaveebide) [kommentaari]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* On nõutav kui koostamise WSFC kobar konfiguratsiooni, kus klaster vajab teisiti IP-aadress on määratud, kuna Azure'i koos oma praeguse funktsionaalsuse soovite määrata kobar nime sama IP-aadressi vastavalt sõlme klaster, luuakse teisiti juhiseid. See tähendab, et käsitsi sammuna tuleb teha klaster eri IP-aadressi määramiseks.
* Kättesaadavus rühma kuulajale saab luua Azure TCP/IP lõpp-punktid, mis on määratud VMs töötab kättesaadavus rühma esmaseid ja teiseseid koopiad.
* Võib olla vaja secure ACL-ID ja nende lõpp-punktid.

[kommentaari]: <>  (TODO vana ajaveeb)
[kommentaari]: <>  (Üksikasjalikud juhised ja vajadused installimist Azure konfigureerimisel AlwaysOn on mugav kõndimisel kaudu kuueosalisest saadaval [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[kommentaari]: <>  (Eelkonfigureeritud AlwaysOn seadistamine Azure Galerii kaudu < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[kommentaari]: <>  (Luua mõne kättesaadavus rühma kuulajale on kõige paremini kirjeldatud [this][virtual-machines-windows-classic-ps-sql-int-listener] õpetus)
[kommentaari]: <>  (Turvamine võrgu lõpp-punktid ACL-ID on selgitatud kõige siin:)
[kommentaari]: <>  (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[kommentaari]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[kommentaari]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[kommentaari]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

On võimalik üle erinevate ka piirkondade Azure SQL serveri kättesaadavusrühma juurutamine. See funktsioon kogusummaks Azure VNet Vnet Ühenduvus ([rohkem üksikasju][virtual-networks-configure-vnet-to-vnet-connection]).
[kommentaari]: <> (TODO vana ajaveebi) [kommentaari]: <> (SQL serveri Kättesaadavusrühmad seadistuse sel juhul on kirjeldatud siin: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>SQL Server Azure'i kõrge saadavus Kokkuvõte
Kuna Azure Storage kaitseb sisu, seal on üks vähem põhjus nõuda kuum puhkerežiimis olev pilt. See tähendab, et kõrge-saadavus stsenaariumist peab kaitsta ainult järgmistel juhtudel:

* Puudumist VM kogu serveri klaster Azure'i või muudel põhjustel hooldustööde tõttu
* SQL serveri eksemplar tarkvara probleemid
* Käsitsi tõrge, kui andmete kustutamine ja taastamine punkti õigel ajal on vajalik kaitsmine

Vaadates vastavaid tehnoloogiad ühte saate leiavad, et kaks esimest juhul võib hõlmata andmebaasi peegeldus või AlwaysOn, kolmas näide ainult võivad hõlmata Logi saatmine.

Vajate keerukamat häälestust AlwaysOn, andmebaasi peegeldus, võrreldes AlwaysOn eeliseid, tasakaalustamiseks. Näiteks võib loetletud need eelised:

*   Teisene loetavad koopiad.
*   Varukoopiate põhjal teisene koopiad.
*   Parem skaleeritavus.
*   Rohkem kui ühe teisene koopiad.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Üldine SQL Server Azure'i Kokkuvõte SAP-i jaoks
On palju soovitused sellest juhendist ja soovitame, et loete seda rohkem kui üks kord enne Azure juurutamise plaanimine. Üldiselt küll, järgige kindlasti ülemise kümme üldine DBMS Azure teatud punktide:

[kommentaari]: <> kui see, mida  (2.3 suurema läbilaskevõime? Kui üks VHD?)
1. Kasutage DBMS uusim versioon, nagu SQL Server 2014, mis on Azure kõige eeliseid. SQL Server, see on SQL Server 2012 SP1 CU4, mis sisaldab toetuseta vastu Azure Storage funktsiooni. Siiski koos SAP-i soovitame vähemalt SQL serveri 2014 SP1 CU1 või SQL Server 2012 SP2 ja uusim CU.
1. Hoolikalt planeerida oma SAP-i süsteemis horisontaalpaigutus Azure tasakaalustamiseks andmete faili paigutuse ja Azure piirangud:
    * Ärge on liiga palju VHDs, kuid on piisavalt, et tagada teie nõutav IOPS jõuate.
    * Pidage meeles, et IOPS on ka piiratud kohta Azure Storage konto ja salvestusruumi kontod piiritletakse sees iga Azure tellimuse ([rohkem üksikasju][azure-subscription-service-limits]). 
    * Ainult triip üle VHDs, kui teil on vaja suurema läbilaskevõimega saavutamiseks.
1. Kunagi tarkvara installimine või sellele faile, mis nõuavad püsimine kettadraivi D:\ on ajutiseks ja midagi sellel leiduvad lähevad kaotsi veebisaidil taaskäivitage Windows.
1. Ärge kasutage Azure'i VHD vahemällu Azure'i Standard Storage.
1. Ärge kasutage Azure geo kopeeritud salvestusruumi kontod.  Kasutage kohalikult liigsete DBMS töökoormus.
1. DBMS tarnija HA/DR lahenduse abil saate andmebaasi andmete korrata.
1. Alati kasutada nimelahendus, ei looda IP-aadressid.
1. Kasutage kõrgeim andmebaasi tihendus võimalik. SQL serveri see on lehe tihendamise.
1. Olge ettevaatlik, SQL serveri piltide kasutamise Azure'i turuplatsilt. Kui kasutate ühte SQL Server, peate muutma eksemplari võrdlemine enne mis tahes SAP NetWeaver süsteemi installimine.
1. Installima ja konfigureerima SAP-i hosti jälgimise Azure, nagu on kirjeldatud [juurutusjuhend] [juurutusjuhend].

## <a name="specifics-to-sap-ase-on-windows"></a>Üksikasjad Windows ASE SAP-i abil
Microsoft Azure'i alustades, saate hõlpsasti migreerida olemasolevaid SAP-i ASE rakendusi Azure'i Virtuaalmasinates. SAP-i ASE Virtual kohapeal võimaldab teil vähendada kogumaksumuse omandiõiguse juurutus, haldamine ja hooldus enterprise laius rakenduste kergesti migreerimine need rakendused Microsoft Azure. SAP-i ASE on Azure virtuaalse masina, kus arendajad ja administraatorite saate siiski kasutada sama arendamise ja haldamise tööriistad, mis on saadaval kohapealse.

On SLA jaoks soovitud Azure'i Virtuaalmasinates, mille saate siit: <https://azure.microsoft.com/support/legal/sla>

Oleme kindel, et Microsoft Azure'i Virtuaalmasinates majutatud täidab hästi võrreldes teiste avaliku pilve virtualization pakkumisi, kuid üksikute tulemused võivad erineda. SAP-i suuruse muutmise SAPS arvude erinevate SAP sertifitseeritud eraldi SAP-i Märkus [1928533]antakse VM SKU-de jaoks.

Laused ja soovitused seoses kasutamine Azure Storage, juurutus, SAP-i VMs või SAP-i jälgimis rakendada juurutustes SAP-i ASE koos SAP-i rakenduste nagu kogu selle dokumendi neli esimest peatükke.

### <a name="sap-ase-version-support"></a>SAP-i ASE versiooni tugi 
SAP praegu toetab SAP-i ASE versioon 16,0 SAP Business Suite koos kasutamiseks. Kõik värskendused ASE SAP-i server või JDBC ja ODBC draiverid koos SAP Business Suite tooted on saadaval ainult läbi SAP-i teenuse turuplatsi veebisaidil: <https://support.sap.com/swdc>.

Nagu installide asutusesiseselt, alla värskendusi ASE SAP-i server või JDBC ja ODBC draivereid otse Sybase'i veebisaiti. Leiate üksikasjalikku teavet plaastrid, mis on toetatud SAP Business Suite toodete asutusesiseste ja Azure'i Virtuaalmasinates kasutada järgmises märkmed SAP-i.

* [1590719]
* [1973241]
 
Üldist teavet SAP Business Suite töötavate SAP-i ASE leiate [SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP-i ASE konfigureerimise juhised SAP-i seotud SAP-i ASE installide Azure'i VM

#### <a name="structure-of-the-sap-ase-deployment"></a>SAP-i ASE juurutamise struktuur
Üldise kirjelduse, tuleks SAP-i ASE täitmisfailid asub või installitud süsteemi draivi soovitud VM base VHD (drive c:\). Tavaliselt enamik SAP-i ASE süsteem ja tööriistad andmebaasid on pole tõesti kasutada raske, SAP NetWeaver töökoormus. Seega jääda C:\drive ka süsteem ja tööriistad andmebaasid (juhtslaidi, mudel, saptools, sybmgmtdb, sybsystemdb). 

Erandi võib olla ajutine andmebaasi kogu töö ja SAP-i ASE, mis mõned SAP ERP ja kõik BW töökoormus korral võib I/O toimingute maht, mis ei sobi algse VM base VHD või suurem andmete maht loodud ajutiste tabelite (drive c:\).
 
Sõltuvalt süsteemi installimiseks kasutatud SAPInst/SWPM versiooni andmebaas võib sisaldada:

* Ühe SAP-i ASE tempdb, mis on loodud SAP-i ASE installimisel
* SAP-i ASE tempdb, mis loodud SAP-i ASE ja on loodud SAP-i installimise tavapärase täiendavad saptempdb
* SAP-i ASE tempdb, mis loodi SAP-i ASE ja täiendavad tempdb, mis on loodud käsitsi (nt järgmised SAP-i Märkus [1752266]) ERP/BW teatud tempdb nõuetele

Teatud ERP- või kõigi BW töökoormus korral on mõistlik, seoses jõudluse hoida tempdb seadmete luuakse Lisaks tempdb (SWPM või käsitsi) ei ole C:\. kui täiendavad tempdb pole olemas, see on soovitatav loomist (SAP-i Märkus [1752266]).

Selliste süsteemide jaoks luuakse Lisaks tempdb tehakse järgmist:

* SAP-i andmebaasi esimene seade esimese tempdb seadme teisaldamine
* Iga VHDs, mis sisaldab seadmetes SAP-i andmebaasi tempdb seadmete lisamine

Selle konfiguratsiooni võimaldab tempdb kas kasutamine rohkem ruumi kui draivi, kuhu on võimalik anda. Abimaterjalina saate kontrollida, tempdb seadme suurused käivitada kohapealse olemasoleva arvutites. Või sellise konfiguratsiooni võimaldaks tempdb, mida ei saa anda draivi IOPS arvud. Uuesti süsteemides, kus töötab kohapealse saab jälgida I/O töökoormus tempdb suhtes.

Kunagi panna mis tahes seadmetes SAP-i ASE peale D:\ ketas VM. See kehtib ka tempdb, isegi kui hoida funktsiooni tempdb objektid on ainult ajutine.

#### <a name="impact-of-database-compression"></a>Andmebaasi tihendamise mõju
Konfiguratsiooni, kus I/O läbilaskevõime võib muutuda piiramine tegurit, võivad aidata iga mõõt, mis vähendab IOPS venitada ühte käitamise IaaS stsenaariumis nagu Azure'i töökoormus. Seetõttu on tungivalt soovitatav veendumaks, et SAP-i ASE tihendamise on kasutanud Azure olemasoleva SAP-i andmebaasi üleslaadimine.

Soovitus tihendamine enne üleslaadimist Azure, kui see pole juba juurutatud sooritamiseks antakse välja mitmel põhjusel:

* Andmete üleslaadimist Azure on väiksem
* Tihendamise täitmise kestus on lühem eeldades, et tugevam riistvara saab kasutada rohkem protsessorid või uuem versioon I/O läbilaskevõime või vähem I/O latentsus kohapealse
* Andmebaasi väiksemad võib kaasa tuua vähem kettaruumi eraldatud kulud

Andmete ja LOB pakkimine töötada majutatud Azure'i Virtuaalmasinates, kuna see kohapealse VM. Vaadake lisateavet selle kohta, kuidas kontrollida, kui tihendamise on juba kasutusel SAP-i ASE olemasolevas andmebaasis SAP-i Märkus [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Andmebaasi eksemplari jälgimiseks DBACockpit abil
SAP-i süsteemidele, mis kasutavad SAP-i ASE andmebaasi platvorm, on DBACockpit juurdepääsetav manustatud brauseriaknad toimingu DBACockpit või Webdynpro. Kuid täisfunktsionaalsuse jälgimise ja haldamise andmebaas on saadaval Webdynpro rakendamisel DBACockpit ainult.

Kui kohapealse süsteemide mitmes etapis peavad kõik SAP NetWeaver funktsiooni kasutatakse funktsiooni DBACockpit Webdynpro rakendamine. Järgige SAP-i Märkus [1245200] lubada webdynpros kasutamist ja luua vajalik need. Kui märkmete ülaltoodud juhiseid tuleb konfigureerida Interneti-side halduri (icm) ka koos pordid kasutatakse http ja https-ühendused. Http vaikesätteks näeb välja umbes järgmine:

> ICMi/server_port_0 = kaitsega = HTTP PORT = 8000 PROCTIMEOUT = 600 AJALÕPP = 600
>
> ICMi/server_port_1 = kaitsega = HTTPS-i PORDI 443$ $, PROCTIMEOUT = = 600 AJALÕPP = 600

ja linke, mis on loodud tehingu DBACockpit näeb välja umbes järgmine:

> https://`<fullyqualifiedhostname`>: 44300/SAP-i/bc/webdynpro/SAP-i/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/SAP-i/bc/webdynpro/SAP-i/dba_cockpit

Sõltuvalt kas ja kuidas süsteemi SAP-i majutusteenuse Azure virtuaalne seade on ühendatud kaudu saidi saidil, mitme saidi või ExpressRoute (asukohaga juurutus), peate veenduge, et ICMI kasutab täielikult kvalifitseeritud hostname, mida saab lahendada arvutis kui proovite avada DBACockpit kaudu. Lugege SAP-i Märkus [773830] mõista, kuidas ICMI määrab täielikult kvalifitseeritud hostinimi sõltuvalt profiili parameetrite ja määrake parameetri ICMi/host_name_full konkreetselt vajadusel.

Kui olete juurutanud vaid stsenaariumis ilma asutusesiseses Ühenduvus kohapealse ja Azure VM, peate määratlema avaliku IP-aadress ja soovitud domainlabel. Vormingu avaliku DNS-i nimi VM siis näeb välja järgmine:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Rohkem üksikasju, mis on seotud DNS-i nimi leiate [siit][virtual-machines-azurerm-versus-azuresm].

SAP-i profiili parameeter ICMi/host_name_full säte Azure VM linki DNS-i nimi võib sarnanema järgmisega:

> https://mydomainlabel.westeurope.cloudapp.net:44300/SAP-i/bc/webdynpro/SAP-i/dba_cockpit

> http://mydomainlabel.westeurope.cloudapp.net:8000/SAP-i/bc/webdynpro/SAP-i/dba_cockpit

Sel juhul peate kindlasti:

* Azure'i portaalis suhelda ICMI pordid TCP/IP võrgu turberühma sissetulevad reeglid lisamine
* Windowsi tulemüüri konfiguratsiooni TCP/IP pordid, suhelda ICMI sissetulevad reeglid lisamine

Automatiseeritud imporditud kõikide paranduste saadaval, on soovitatav perioodiliselt rakendamiseks paranduse saidikogumi SAP-i teate oma SAP-i versiooni:

* [1558958]
* [1619967]
* [1882376]

SAP-i ASE DBA kokpiti Lisateavet leiate järgmistest SAP-i märkmete:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP-i ASE varundamise ja taastamise kaalutlused
SAP-i ASE Azure juurutamisel oma varukoopia meetodi läbi vaadata. Isegi juhul, kui süsteemi ei ole produktiivne süsteemi, SAP-i andmebaasi korraldaja SAP-i ASE tuleb varundada perioodiliselt. Azure Storage hoiab kolm pilti, seetõttu on nüüd varukoopia väiksemaid osas kompensatsiooni salvestusruumi krahh. Esmane põhjus säilitades proper varundus ja taaste leping on rohkem, et saate esitada punkti kellaaja funktsioonid loogiline/käsitsi tõrkeid kompenseeri. Nii, et eesmärk on kas kasutamine varukoopiate taastada andmebaas on teatud aja või kasutada varukoopiaid Azure seeme teise süsteemi olemasoleva andmebaasi kopeerimise abil. Näiteks võib kannate 2-astme SAP-i konfigureerimine 3 süsteemi häälestus sama süsteemi järgi taastamine varukoopia.

Varundus ja taaste andmebaasi Azure töötab samamoodi nagu kohapealse. SAP-i märkmed leiate järgmistest teemadest:

* [1588316]
* [1585981]

üksikasjade loomise dump konfiguratsioone ja ajastamise varukoopiad. Sõltuvalt teie strateegia ja saate konfigureerida vajadustele andmebaas ja Logi puistab peale üks olemasoleva VHDs kettale või lisada ka täiendavad VHD jaoks varukoopia.  Vähendada tõrke korral andmete kaotsimineku ohtu on soovitatav kasutada on VHD, kus asub pole andmebaasi seade.

Lisaks andmete ja LOB tihendamise SAP-i ASE pakub varukoopia tihendamise. Hõivata vähem ruumi puistab andmebaas ja Logi koos on soovitatav kasutada varukoopia tihendamise. Lugege lisateavet SAP-i Märkus [1588316] . Tihendamise varundamine on oluline vähendada andmeid, mida kui plaanite varukoopiate või VHDs sisaldava varukoopia puistab: Azure'i virtuaalse masina kohapealse alla laadida.

Ärge kasutage ketas D:\ andmebaasi või log dump sihtkohana.

#### <a name="performance-considerations-for-backupsrestores"></a>Varukoopiate/taastab kohta
Nagu tühjal metallist juurutuste varundus ja taaste jõudlus sõltub mitu mahud saab lugeda paralleelselt ja nende mahud läbilaskevõime võib olla. Lisaks kasutavad varukoopia tihendamise CPU tarbimine võib mängida olulised VMs koos ainult kuni 8 CPU teemad. Seetõttu üks täita:

* Vähem soovitud arv VHDs kasutada andmebaasi seadmed, talletamiseks väiksemate üldine jõudlus lugemine
* Väiksemate arv Teemad VM rohkem raske varukoopia tihendamise mõju
* Vähem sihtkohtade (triip kataloogide, VHDs) kirjutamise varukoopia, olenevalt sellest, kumb on läbilaskevõime

Suurendamiseks sihtmärgid on kaks kirjutamise suvandid, mida saab kasutada/kombineeritud vastavalt vajadusele:

* Triipude töötlemine varukoopia target helitugevuse üle mitme ühendatud VHDs IOPS läbilaskevõime musta Triibutatud draivile parandamiseks
* SAP-i ASE tasemel, mis kasutab rohkem kui üks siht kataloogi kirjutamiseks dump, et dump konfiguratsiooni loomine

Triipude töötlemine maht üle mitme ühendatud VHDs on on kirjeldatud varem sellest juhendist. SAP-i ASE dump konfiguratsiooni mitme kataloogide kasutamise kohta lisateabe saamiseks vaadake salvestatud protseduur sp_config_dump [Sybase'i info Center](http://infocenter.sybase.com/help/index.jsp)dump konfiguratsiooni loomiseks kasutatava dokumentatsioon.

### <a name="disaster-recovery-with-azure-vms"></a>Avariitaastet Azure vms

#### <a name="data-replication-with-sap-sybase-replication-server"></a>SAP-i Sybase'i Dispersioonanalüüs serveriga andmete kopeerimine
SAP-i Sybase'i Dispersioonanalüüs Server (SRS) SAP-i ASE pakub soe valige lahenduse edastamiseks andmebaasitoiminguid asünkroonselt kauges asukohta. 

Installimine ja käitamine SRS töötab ka funktsionaalselt majutatud Azure virtuaalse masina teenuste kohapealse tagamisse VM.

Tulevaste väljalasete koos kavandatakse ASE HADR SAP-i kopeerimine serveri kaudu. Seda katsetada ja Microsoft Azure'i platvormid välja niipea, kui see on saadaval.

## <a name="specifics-to-sap-ase-on-linux"></a>SAP-i ASE Linuxi abil üksikasjad

Microsoft Azure'i alustades, saate hõlpsasti migreerida olemasolevaid SAP-i ASE rakendusi Azure'i Virtuaalmasinates. SAP-i ASE Virtual kohapeal võimaldab teil vähendada kogumaksumuse omandiõiguse juurutus, haldamine ja hooldus enterprise laius rakenduste kergesti migreerimine need rakendused Microsoft Azure. SAP-i ASE on Azure virtuaalse masina, kus arendajad ja administraatorite saate siiski kasutada sama arendamise ja haldamise tööriistad, mis on saadaval kohapealse.

Azure VMs kasutamise kohta on oluline teada ametlik SLAs, mille saate siit: <https://azure.microsoft.com/support/legal/sla>

SAP-i Märkus [1928533]pakutakse SAP-i sizing teavet ja SAP-i sertifitseeritud VM SKU-de jaoks loendit. Täiendavad SAP-i sortimine dokumentide Azure virtuaalse masinad leiate siit <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> ja siin <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Laused ja soovitused seoses kasutamine Azure Storage, juurutus, SAP-i VMs või SAP-i jälgimis rakendada juurutustes SAP-i ASE koos SAP-i rakenduste nagu kogu selle dokumendi neli esimest peatükke.

Järgmised kaks SAP-i märkmeid lisada üldteavet ASE ASE ja Linux pilves:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>SAP-i ASE versiooni tugi 
SAP praegu toetab SAP-i ASE versioon 16,0 SAP Business Suite koos kasutamiseks. Kõik värskendused ASE SAP-i server või JDBC ja ODBC draiverid koos SAP Business Suite tooted on saadaval ainult läbi SAP-i teenuse turuplatsi veebisaidil: <https://support.sap.com/swdc>.

Nagu installide asutusesiseselt, alla värskendusi ASE SAP-i server või JDBC ja ODBC draivereid otse Sybase'i veebisaiti. Leiate üksikasjalikku teavet plaastrid, mis on toetatud SAP Business Suite toodete asutusesiseste ja Azure'i Virtuaalmasinates kasutada järgmises märkmed SAP-i.

* [1590719]
* [1973241]
 
Üldist teavet SAP Business Suite töötavate SAP-i ASE leiate [SCN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>SAP-i ASE konfigureerimise juhised SAP-i seotud SAP-i ASE installide Azure'i VM

#### <a name="structure-of-the-sap-ase-deployment"></a>SAP-i ASE juurutamise struktuur
Üldise kirjelduse, tuleks SAP-i ASE täitmisfailid asub või installitud juurkausta failisüsteemi VM (/sybase). Tavaliselt enamik SAP-i ASE süsteem ja tööriistad andmebaasid on pole tõesti kasutada raske, SAP NetWeaver töökoormus. Seega süsteem ja tööriistad andmebaasid (juhtslaidi, mudel, saptools, sybmgmtdb, sybsystemdb) võib jääda ka root failisüsteemis. 

Erandi võib olla kõik töö ja SAP-i ASE, mis mõned SAP ERP ja kõik BW töökoormus korral võib I/O toimingute maht, mis ei sobi algse VM OS kettale või suurem andmete maht loodud ajutiste tabelite ajutist andmebaasi.
 
Sõltuvalt süsteemi installimiseks kasutatud SAPInst/SWPM versiooni andmebaas võib sisaldada:

* Ühe SAP-i ASE tempdb, mis on loodud SAP-i ASE installimisel
* SAP-i ASE tempdb, mis loodud SAP-i ASE ja on loodud SAP-i installimise tavapärase täiendavad saptempdb
* SAP-i ASE tempdb, mis loodi SAP-i ASE ja täiendavad tempdb, mis on loodud käsitsi (nt järgmised SAP-i Märkus [1752266]) ERP/BW teatud tempdb nõuetele

Teatud ERP- või kõigi BW töökoormus korral on mõistlik, seoses jõudluse hoida eraldi failisüsteemis, mis saab esindatud ühe Azure'i andmed kettale või Linuxi RAID kestvad mitme Azure'i andmed ketast tempdb seadmete luuakse Lisaks tempdb (SWPM või käsitsi). Kui pole täiendavad tempdb on olemas, see on soovitatav loomist (SAP-i Märkus [1752266]).

Selliste süsteemide jaoks luuakse Lisaks tempdb tehakse järgmist:

* SAP-i andmebaasi esimese failisüsteemi esimese tempdb kataloogi teisaldamine
* Iga VHDs, mis sisaldab failisüsteemi SAP-i andmebaasi tempdb kataloogide lisamine

Selle konfiguratsiooni võimaldab tempdb kas kasutamine rohkem ruumi kui draivi, kuhu on võimalik anda. Abimaterjalina saate kontrollida, tempdb directory suurused käivitada kohapealse olemasoleva arvutites. Või sellise konfiguratsiooni võimaldaks tempdb, mida ei saa anda draivi IOPS arvud. Uuesti süsteemides, kus töötab kohapealse saab jälgida I/O töökoormus tempdb suhtes.

Ärge pange mis tahes SAP-i ASE kataloogide/mnt või /mnt/resource VM. See kehtib ka tempdb, isegi kui hoida funktsiooni tempdb objektide on ajutine Kuna/mnt või /mnt/resource on vaikimisi Azure VM temp ruumi, mis ei ole püsiv. Täpsemat teavet Azure VM temp ruumi leiate [selle] artikli teemad[virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Andmebaasi tihendamise mõju
Konfiguratsiooni, kus I/O läbilaskevõime võib muutuda piiramine tegurit, võivad aidata iga mõõt, mis vähendab IOPS venitada ühte käitamise IaaS stsenaariumis nagu Azure'i töökoormus. Seetõttu on tungivalt soovitatav veendumaks, et SAP-i ASE tihendamise on kasutanud Azure olemasoleva SAP-i andmebaasi üleslaadimine.

Soovitus tihendamine enne üleslaadimist Azure, kui see pole juba juurutatud sooritamiseks antakse välja mitmel põhjusel:

* Andmete üleslaadimist Azure on väiksem
* Tihendamise täitmise kestus on lühem eeldades, et tugevam riistvara saab kasutada rohkem protsessorid või uuem versioon I/O läbilaskevõime või vähem I/O latentsus kohapealse
* Andmebaasi väiksemad võib kaasa tuua vähem kettaruumi eraldatud kulud

Andmete ja LOB pakkimine töötada majutatud Azure'i Virtuaalmasinates, kuna see kohapealse VM. Vaadake lisateavet selle kohta, kuidas kontrollida, kui tihendamine on juba kasutusel SAP-i ASE olemasolevas andmebaasis SAP-i Märkus [1750510]. Vt ka SAP-i Märkus [2121797] lisateabe saamiseks andmebaasi tihendamise kohta.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Andmebaasi eksemplari jälgimiseks DBACockpit abil
SAP-i süsteemidele, mis kasutavad SAP-i ASE andmebaasi platvorm, on DBACockpit juurdepääsetav manustatud brauseriaknad toimingu DBACockpit või Webdynpro. Kuid täisfunktsionaalsuse jälgimise ja haldamise andmebaas on saadaval Webdynpro rakendamisel DBACockpit ainult.

Kui kohapealse süsteemide mitmes etapis peavad kõik SAP NetWeaver funktsiooni kasutatakse funktsiooni DBACockpit Webdynpro rakendamine. Järgige SAP-i Märkus [1245200] lubada webdynpros kasutamist ja luua vajalik need. Kui märkmete ülaltoodud juhiseid tuleb konfigureerida Interneti-side halduri (icm) ka koos pordid kasutatakse http ja https-ühendused. Http vaikesätteks näeb välja umbes järgmine:

> ICMi/server_port_0 = kaitsega = HTTP PORT = 8000 PROCTIMEOUT = 600 AJALÕPP = 600
>
> ICMi/server_port_1 = kaitsega = HTTPS-i PORDI 443$ $, PROCTIMEOUT = = 600 AJALÕPP = 600

ja linke, mis on loodud tehingu DBACockpit näeb välja umbes järgmine:

> https://`<fullyqualifiedhostname`>: 44300/SAP-i/bc/webdynpro/SAP-i/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/SAP-i/bc/webdynpro/SAP-i/dba_cockpit

Sõltuvalt kas ja kuidas süsteemi SAP-i majutusteenuse Azure virtuaalne seade on ühendatud kaudu saidi saidil, mitme saidi või ExpressRoute (asukohaga juurutus), peate veenduge, et ICMI kasutab täielikult kvalifitseeritud hostname, mida saab lahendada arvutis kui proovite avada DBACockpit kaudu. Lugege SAP-i Märkus [773830] mõista, kuidas ICMI määrab täielikult kvalifitseeritud hostinimi sõltuvalt profiili parameetrite ja määrake parameetri ICMi/host_name_full konkreetselt vajadusel.

Kui olete juurutanud vaid stsenaariumis ilma asutusesiseses Ühenduvus kohapealse ja Azure VM, peate määratlema avaliku IP-aadress ja soovitud domainlabel. Vormingu avaliku DNS-i nimi VM siis näeb välja järgmine:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Rohkem üksikasju, mis on seotud DNS-i nimi leiate [siit][virtual-machines-azurerm-versus-azuresm].

SAP-i profiili parameeter ICMi/host_name_full säte Azure VM linki DNS-i nimi võib sarnanema järgmisega:

> https://mydomainlabel.westeurope.cloudapp.net:44300/SAP-i/bc/webdynpro/SAP-i/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.net:8000/SAP-i/bc/webdynpro/SAP-i/dba_cockpit

Sel juhul peate kindlasti:

* Azure'i portaalis suhelda ICMI pordid TCP/IP võrgu turberühma sissetulevad reeglid lisamine
* Windowsi tulemüüri konfiguratsiooni TCP/IP pordid, suhelda ICMI sissetulevad reeglid lisamine

Automatiseeritud imporditud kõikide paranduste saadaval, on soovitatav perioodiliselt rakendamiseks paranduse saidikogumi SAP-i teate oma SAP-i versiooni:

* [1558958]
* [1619967]
* [1882376]

SAP-i ASE DBA kokpiti Lisateavet leiate järgmistest SAP-i märkmete:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>SAP-i ASE varundamise ja taastamise kaalutlused
SAP-i ASE Azure juurutamisel oma varukoopia meetodi läbi vaadata. Isegi juhul, kui süsteemi ei ole produktiivne süsteemi, SAP-i andmebaasi korraldaja SAP-i ASE tuleb varundada perioodiliselt. Azure Storage hoiab kolm pilti, seetõttu on nüüd varukoopia väiksemaid osas kompensatsiooni salvestusruumi krahh. Esmane põhjus säilitades proper varundus ja taaste leping on rohkem, et saate esitada punkti kellaaja funktsioonid loogiline/käsitsi tõrkeid kompenseeri. Nii, et eesmärk on kas kasutamine varukoopiate taastada andmebaas on teatud aja või kasutada varukoopiaid Azure seeme teise süsteemi olemasoleva andmebaasi kopeerimise abil. Näiteks võib kannate 2-astme SAP-i konfigureerimine 3 süsteemi häälestus sama süsteemi järgi taastamine varukoopia.

Varundus ja taaste andmebaasi Azure töötab samamoodi nagu kohapealse. SAP-i märkmed leiate järgmistest teemadest:

* [1588316]
* [1585981]

üksikasjade loomise dump konfiguratsioone ja ajastamise varukoopiad. Sõltuvalt teie strateegia ja saate konfigureerida vajadustele andmebaas ja Logi puistab peale üks olemasoleva VHDs kettale või lisada ka täiendavad VHD jaoks varukoopia.  Vähendada tõrke korral andmete kaotsimineku ohtu on soovitatav kasutada on VHD, kus pole andmebaasi directory/fail asub.

Lisaks andmete ja LOB tihendamise SAP-i ASE pakub varukoopia tihendamise. Hõivata vähem ruumi puistab andmebaas ja Logi koos on soovitatav kasutada varukoopia tihendamise. Lugege lisateavet SAP-i Märkus [1588316] . Tihendamise varundamine on oluline vähendada andmeid, mida kui plaanite varukoopiate või VHDs sisaldava varukoopia puistab: Azure'i virtuaalse masina kohapealse alla laadida.

Ärge kasutage Azure VM ruumi temp/mnt või /mnt/resource andmebaasi või log dump sihtkohana.

#### <a name="performance-considerations-for-backupsrestores"></a>Varukoopiate/taastab kohta
Nagu tühjal metallist juurutuste varundus ja taaste jõudlus sõltub mitu mahud saab lugeda paralleelselt ja nende mahud läbilaskevõime võib olla. Lisaks kasutavad varukoopia tihendamise CPU tarbimine võib mängida olulised VMs koos ainult kuni 8 CPU teemad. Seetõttu üks täita:

* Vähem soovitud arv VHDs kasutada andmebaasi seadmed, talletamiseks väiksemate üldine jõudlus lugemine
* Väiksemate arv Teemad VM rohkem raske varukoopia tihendamise mõju
* Kirjutage varukoopia, olenevalt sellest, kumb on läbilaskevõime vähem sihtkohtade (Linux tarkvara RAID, VHDs)

Suurendamiseks sihtmärgid on kaks kirjutamise suvandid, mida saab kasutada/kombineeritud vastavalt vajadusele:

* Triipude töötlemine varukoopia target helitugevuse üle mitme ühendatud VHDs IOPS läbilaskevõime musta Triibutatud draivile parandamiseks
* SAP-i ASE tasemel, mis kasutab rohkem kui üks siht kataloogi kirjutamiseks dump, et dump konfiguratsiooni loomine

Triipude töötlemine maht üle mitme ühendatud VHDs on on kirjeldatud varem sellest juhendist. SAP-i ASE dump konfiguratsiooni mitme kataloogide kasutamise kohta lisateabe saamiseks vaadake salvestatud protseduur sp_config_dump [Sybase'i info Center](http://infocenter.sybase.com/help/index.jsp)dump konfiguratsiooni loomiseks kasutatava dokumentatsioon.

### <a name="disaster-recovery-with-azure-vms"></a>Avariitaastet Azure vms

#### <a name="data-replication-with-sap-sybase-replication-server"></a>SAP-i Sybase'i Dispersioonanalüüs serveriga andmete kopeerimine
SAP-i Sybase'i Dispersioonanalüüs Server (SRS) SAP-i ASE pakub soe valige lahenduse edastamiseks andmebaasitoiminguid asünkroonselt kauges asukohta. 

Installimine ja käitamine SRS töötab ka funktsionaalselt majutatud Azure virtuaalse masina teenuste kohapealse tagamisse VM.

SAP-i kopeerimine serveri kaudu ASE HADR sel hetkel ei toetata. See võib katsetada ja välja Microsoft Azure'i platvormid tulevikus.

## <a name="specifics-to-oracle-database-on-windows"></a>Üksikasjad Windows Oracle'i andmebaasiga
Midyear 2013, kuna Oracle'i tarkvara toetab Oracle'i käivitada Microsoft Windowsi Hyper-V ja Azure. Lugege seda artiklit saada lisateavet Windows Hyper-V ja Azure üldine toetamine Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Pärast üldist tugi, SAP-i rakenduste kasutamine Oracle'i andmebaaside konkreetse stsenaariumi toetatud on ka. Selle dokumendi osa nimega üksikasjad.

### <a name="oracle-version-support"></a>Oracle'i versiooni tugi
Oracle'i versioonid ja vastavaid OS versioonid, mis on toetatud Oracle'i Azure'i Virtuaalmasinates SAP-i töötavate kõik üksikasjad leiate järgmised SAP-i Märkus [2039619]

Üldist teavet selle kohta, kuidas SAP Business Suite Oracle leiate SCN: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle'i konfigureerimise juhised SAP-i installide Azure VM

#### <a name="storage-configuration"></a>Salvestusruumi konfigureerimine
Ainult ühe eksemplari Oracle'i abil vormindatud NTFS ketast on toetatud. Kõigi andmebaasi faile hoitakse põhjal VHD ketast NTFS failisüsteemis. Nende VHDs on paigaldatud Azure VM ja põhinevad Azure lehe bloobimälu (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Mis tahes tüüpi võrgu kaudu või remote ühtlasi nagu Azure faili teenuseid:
 
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-files.aspx>
 
**pole** toetatud Oracle'i andmebaasi failide!

Azure'i VHDs põhjal lehe Azure'i bloobimälu kasutamisel laused tehtud selles dokumendis peatüki [vahemälu VMs ja VHDs] [dbms-juhend-2.1] ja [Microsoft Azure'i Tabelimäluga] [dbms-juhend-2.3] rakendamine juurutuste ka Oracle'i andmebaasiga.

Nagu varem üldine dokumendi osa, kvootide IOPS läbilaskevõime Azure'i VHDs jaoks on olemas. Täpse kvootide sõltuvalt VM kasutatakse. VM tüüpide koos oma loendi leiate [siit][virtual-machines-sizes]

Toetatud failitüübid Azure VM tuvastamiseks vaadake SAP-i Märkus [1928533]

Kui praegune IOPS kvoodi kohta ketta vastab nõuetele, on võimalik kõik ühe ühe ühendatud Azure'i VHD DB failid talletada. 

Kui nõutakse veel IOPS, on tungivalt soovitatav akna salvestusruumi kaustu (ainult Windows Server 2012 saadaval ja kõrgem) või Windowsi striping Windows 2008 R2 abil saate luua ühe suure loogilise seadme üle mitme ühendatud VHD ketast. Vt ka selle dokumendi Peatükk [tarkvara RAID] [dbms-juhend-2.2]. Seda moodust lihtsustab selle Administreerimine elemente, et hallata kettaruumi ja vaeva käsitsi levitamist failid üle mitme ühendatud VHDs hoiab ära.

#### <a name="backup--restore"></a>Varundamine ja taastamine
Varundamiseks / taastada funktsioone, SAP-i BR * tööriistad Oracle on toetatud samamoodi nagu standard Windows Serveri opsüsteemid ja Hyper-V. Varufailide kettale ja taastada kettalt toetatakse ka Oracle'i taastamine Manager (RMAN).

#### <a name="high-availability"></a>Kõrge-saadavus
[kommentaari]: <>  (link viitab ASM)
Oracle'i andmete Guard on toetatud kõrge-saadavus ja katastroofi taastamise eesmärgil. Üksikasjad leiate [selle] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentatsiooni.

#### <a name="other"></a>Muud
Kõik muud üldised nagu Azure-saadavus komplektid või SAP-i jälgimine rakendada selle dokumendi juurutuste VMs Oracle'i andmebaasiga ka esimesed kolm peatükke kirjeldatud.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>SAP-i MaxDB andmebaasi Windows üksikasjad

### <a name="sap-maxdb-version-support"></a>SAP-i MaxDB versiooni tugi
SAP-i toetab praegu SAP-i MaxDB versioon 7,9 koos kasutamiseks SAP NetWeaver põhineva Azure. Ainult läbi SAP-i teenuse turuplatsi veebisaidil <https://support.sap.com/swdc>on olemas kõik MaxDB SAP-i server või JDBC ja ODBC draiverid koos SAP NetWeaver toodete värskendused.
Üldist teavet SAP NetWeaver töötavate SAP-i MaxDB leiate <https://scn.sap.com/community/maxdb>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>SAP-i MaxDB DBMS toetatud Microsoft Windowsi versiooni ja Azure VM tüübid
SAP-i MaxDB DBMS Azure toetatud Microsoft Windowsi versiooni leiate järgmistest teemadest.

* [SAP-i toote kättesaadavus maatriksi (PAM)][sap-pam]
* SAP-i Märkus [1928533]

See on soovitatav kasutada operatsioonisüsteemi Microsoft Windows, mis on Microsoft Windows 2012 R2 uusim versioon.

### <a name="available-sap-maxdb-documentation"></a>Saadaval SAP-i MaxDB dokumentatsioon
SAP-i MaxDB dokumentatsiooni värskendatud loendi leiate järgmised SAP-i Märkus [767598]
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP-i MaxDB konfigureerimise juhised SAP-i installide Azure VM

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Salvestusruumi konfigureerimine
Azure'i salvestusruumi head tavad SAP-i MaxDB soovituste üldist mainitud Peatükk [struktuuri RDBMS juurutamise] [dbms-juhend-2].

> [AZURE.IMPORTANT] SAP-i MaxDB on muid andmebaase, nagu ka andmed ja logifailide. SAP-i MaxDB terminoloogia õige termin on siiski "mahu" (mitte "faili"). Näiteks on SAP-i MaxDB andmemahtudega ja Logi maht. Ärge ajage neid OS ketta maht. 

Lühidalt peate tegema järgmist.

* Määrake Azure storage konto, mis hoiab SAP-i MaxDB andmete ja logide mahud (st failid) **Kohaliku liigsete salvestusruumi (LRS)** määratletud Peatükk [Microsoft Azure'i Tabelimäluga] [dbms-juhend-2.3] nimega.
* Eraldi IO tee SAP-i MaxDB andmemahtudega (st failid) IO tee Logi maht (st failid). See tähendab, et SAP-i MaxDB andmemahtudega (st failid) on üks loogiline ketas olema installitud ja SAP-i MaxDB log mahud (st failid) on olema installitud loogilise teisele kettale.
* Määrake soovitud proper faili vahemällu puhul iga Azure'i bloobimälu, olenevalt sellest, kas kasutate seda SAP-i MaxDB andmeid või log mahud (st failid) ja kas kasutada Azure Standard- või Azure Premium Storage, mida on kirjeldatud [vahemälu vms] [dbms-juhend-2.1].
* Kui praegune IOPS kvoodi kohta ketta vastab nõuetele, on võimalik talletada kõik andmemahtudega ühe ühendatud Azure'i VHD ja salvestada kõik andmebaasi Logi maht ka mõne muu ühe ühendatud Azure'i VHD.
* Kui nõutakse veel IOPS ja/või ruumi, on tungivalt soovitatav üle mitme ühendatud VHD ketast ühe suure loogilise seadme loomiseks kasutada Microsoft akna salvestusruumi kaustu (ainult saadaval Microsoft Windows Server 2012 ja uuemad versioonid) või Microsoft Windows striping Microsoft Windows 2008 R2. Vt ka selle dokumendi Peatükk [tarkvara RAID] [dbms-juhend-2.2]. Seda moodust lihtsustab selle Administreerimine elemente, et hallata kettaruumi ja vaeva käsitsi levitamise failid üle mitme ühendatud VHDs hoiab ära.
* Nõuded kõrgeim IOPS saate kasutada Azure Premium Storage, mis on saadaval DS-sari ja GS-sarja VMs.

![Azure'i IaaS VM jaoks SAP-i MaxDB DBMS viide konfigureerimine][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Varundus ja taaste
SAP-i MaxDB juurutamisel Azure'i sisse peab teil läbi vaadata oma varukoopia meetod. Isegi juhul, kui süsteemi ei ole produktiivne süsteemi, SAP-i andmebaasi korraldaja SAP-i MaxDB tuleb varundada perioodiliselt. Azure Storage hoiab kolm pilti, seetõttu on nüüd varukoopia vähem oluline kaitsta teie süsteemi vastu salvestamise tõrge ja tähtsamatele funktsionaalseid või haldus vigade osas. Esmane põhjus säilitades proper varundus ja taaste leping on nii, et saate kompenseeri loogiline või käsitsi tõrkeid funktsioonid punkti õigel ajal esitada. Nii, et eesmärk on kasutama varukoopiate andmebaasi taastamiseks teatud punkti ajas või varukoopiaid kasutamine Azure seeme teise süsteemi olemasoleva andmebaasi kopeerimise abil. Näiteks võib kannate 2-astme SAP-i konfigureerimine 3 süsteemi häälestus sama süsteemi järgi taastamine varukoopia.

Varundus ja taaste andmebaasi Azure töötab samamoodi nagu see kohapealse süsteemide, abil saate standardse SAP-i MaxDB varundus ja taaste tööriistad, mis on kirjeldatud SAP-i MaxDB dokumentatsiooni dokumentide loendis SAP-i Märkus [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Varundus ja taaste kohta
Nagu tühjal metallist juurutuste varundus ja taaste jõudlus sõltub mitu mahud saab lugeda samal ajal ja läbilaskevõime nende mahtu. Lisaks CPU tarbimine kasutatavaid varukoopia tihendamise saate mängida olulised VMs koos kuni 8 CPU teemad. Seetõttu üks täita:

* Vähem soovitud arvu VHDs talletatakse andmebaasi seadmed, madalam üldine loetuks jõudlus
* Väiksemate arv Teemad VM rohkem raske varukoopia tihendamise mõju
* Vähem sihtkohtade (triip kataloogide, VHDs) kirjutamise varukoopia, väiksem on läbilaskevõime

Sihtmärgid kirjutamise suurendamiseks on kaks võimalust, mida saate kasutada, võib-olla kombinatsioon, sõltuvalt teie vajadustele.

* Pühendades eraldi mahud varundamine
* Triipude töötlemine varukoopia target helitugevuse üle mitme ühendatud VHDs IOPS läbilaskevõime selle musta Triibutatud draivil parandamiseks
* Kas teil on eraldi sihtotstarbeline loogiline ketas seadmete jaoks
    * SAP-i MaxDB varukoopia maht (st failid)
    * SAP-i MaxDB andmemahtudega (st failid)
    * SAP-i MaxDB Logi maht (st failid)

Triipude töötlemine maht üle mitme ühendatud VHDs on on kirjeldatud varem selle dokumendi Peatükk [tarkvara RAID] [dbms-juhend-2.2]. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Muud
Kõik muud üldised näiteks Azure-saadavus komplektid või SAP-i järelevalvet rakendada ka kirjeldatud kolm esimest selles dokumendis juurutuste VMs MaxDB SAP-i andmebaasiga peatükke.
Muud SAP-i MaxDB kohased sätted on läbipaistvaid Azure vms ja on kirjeldatud eri dokumentide SAP-i Märkus [767598] ja SAP-i juhised:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>SAP-i liveCache Windows jaoks üksikasjad

### <a name="sap-livecache-version-support"></a>SAP-i liveCache versiooni tugi
SAP-i liveCache toetatud Azure'i Virtuaalmasinates minimaalsete versioon on **SAP-i LC/LCAPPS 10.0 SP 25** **liveCache 7.9.08.31** ja **hoiuala-koostamine 25**, **EhP** 2 SAP SCM 7.0 ja kõrgemale välja.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>SAP-i liveCache DBMS toetatud Microsoft Windowsi versiooni ja Azure VM tüübid
SAP-i liveCache Azure toetatud Microsoft Windowsi versiooni leiate järgmistest teemadest.

* [SAP-i toote kättesaadavus maatriksi (PAM)][sap-pam]
* SAP-i Märkus [1928533]

See on soovitatav kasutada operatsioonisüsteemi Microsoft Windows, mis on Microsoft Windows 2012 R2 uusim versioon. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP-i liveCache konfigureerimise juhised SAP-i installide Azure'i VM

#### <a name="recommended-azure-vm-types"></a>Soovitatav Azure'i VM tüübid
SAP-i liveCache on rakendus, mis teeb suur arvutused, summa ja kiiruse RAM ja CPU on suur mõju SAP-i liveCache jõudlust. 

Azure'i VM puhul, mis ei toeta SAP-i (SAP-i Märkus [1928533]), on kõik virtuaalse CPU ressursse, mis on eraldatud VM tagatud sihtotstarbeline füüsilise CPU ressursse selle hypervisor. Toimub pole overprovisioning (ja seega ei ole konkurentsiga CPU ressursid).

Samuti on VM mälu Azure VM eksemplari kõigi jaoks ei toeta SAP-i, ei kasutata vastendatud füüsilise mälu – overprovisioning (üle kohustuse), näiteks 100%.

See seisukohalt on äärmiselt soovitatav kasutada uut D-sarjade või DS-sarja (kombinatsioonis Azure Premium Storage) Azure VM tüüp, kui nad on 60% kiiremini protsessorite kui A-seeria. Kõrgeim RAM ja CPU koormus, saate kasutada G-sari ja GS-sarja (kombinatsioonis Azure Premium Storage) VMs uusima Inteli® Xeon® protsessor E5 v3 pere, mis on kaks korda mälu ja neli korda ühtlase olekus ketas talletamist (SSD) D/DS-sarja.

#### <a name="storage-configuration"></a>Salvestusruumi konfigureerimine
SAP-i liveCache põhineb SAP-i MaxDB tehnoloogia, Azure storage head tava soovitused mainitud SAP-i MaxDB peatüki [salvestusruumi konfiguratsiooni] [dbms-juhend-8.4.1] sobivad ka SAP-i liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Sihtotstarbeline Azure VM liveCache jaoks
Nagu SAP-i liveCache kasutab intensiivselt arvutusliku, produktiivne kasutus on väga soovitatav juurutamise kohta asjakohast Azure virtuaalse masina. 
 
![Spetsiaalne Azure VM liveCache puhul kasutada jaoks][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Varundus ja taaste
Varundus ja taaste, sh jõudluse huvides, on juba kirjeldatud oluline SAP-i MaxDB lõikudesse [varundus ja taaste] [dbms-juhend-8.4.2] ja [jõudluse kaalutluste kohta varundus ja taaste] [dbms-juhend-8.4.3]. 

#### <a name="other"></a>Muud
Kõik muud üldised on juba kirjeldatud oluline SAP-i MaxDB [see] [dbms-juhend-8.4.4] Peatükk. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Üksikasjad Windows Server SAP-i sisu jaoks
SAP-i sisu Server on eraldi, Serveri komponent sisu, näiteks e-dokumentide salvestamine muus vormingus. SAP-i serveri ei tehnoloogia areng ja peab olema kasutatud rist-taotlus SAP-i rakendusi. See on installitud eraldi süsteemi. Tüüpilised sisu on koolitusmaterjalid ja dokumendid teadmisi ladu või tehnilised joonised pärinevate mySAP PLM dokumendihalduse süsteem. 

### <a name="sap-content-server-version-support"></a>SAP-i serveri versiooni tugi
SAP-i, toetab praegu.

* **SAP-i serveri** versioon **6.50 (ja uuemad versioonid)**
* **SAP-i MaxDB 7,9 versioon**
* **Microsoft IIS (Internet Information Server) versiooni 8.0 (ja uuemad versioonid)**

See on soovitatav kasutada SAP-i sisu Server, mis selle dokumendi kirjutamise ajal on **6,50 SP4**, uusim versioon ja **8.5 Microsoft IIS-i**uusim versioon. 

Märkige ruut Viimane toetatud versioonid SAP-i serveri ja [SAP-i kättesaadavus maatriksi (PAM)]rakenduses Microsoft IIS[sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>SAP-i serveri toetatud Microsoft Windowsis ja Azure VM tüübid
SAP-i serveri Azure toetatud Windowsi versiooni leiate järgmistest teemadest.

* [SAP-i toote kättesaadavus maatriksi (PAM)][sap-pam]
* SAP-i Märkus [1928533]

See on soovitatav kasutada Microsoft Windows, mis on **Windows Server 2012 R2**selle dokumendi kirjutamise ajal uusim versioon.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP-i serveri konfigureerimise juhised SAP-i installide Azure VM

#### <a name="storage-configuration"></a>Salvestusruumi konfigureerimine 
Kui konfigureerite SAP-i sisu serveri andmebaasis SAP-i MaxDB faile talletada, kõik Azure storage head tavad soovitus mainitud SAP-i MaxDB peatüki [salvestusruumi konfiguratsiooni] [dbms-juhend-8.4.1] sobivad ka SAP-i serveri stsenaariumi. 

Kui konfigureerite SAP-i serveri failisüsteemi failide talletamiseks, on soovitatav kasutada sihtotstarbeline loogiline ketas. Kasutades Salvestusruum võimaldab teil suurendada ka loogilise ketta suurus ja IOPS läbilaskevõime, peatükk [tarkvara RAID] [dbms-juhend-2.2] kirjeldatud. 

#### <a name="sap-content-server-location"></a>SAP-i sisu serveri asukoht
SAP-i sisu Server on sama Azure piirkond ja Azure VNET, kus on juurutatud SAP süsteemi kasutusele võtta. Olete ise otsustada, kas soovite SAP-i serveri komponendid sihtotstarbeline Azure VM või sama VM, kus töötab SAP süsteemi juurutamine. 
 
![SAP-i sisu serveri sihtotstarbeline Azure'i VM][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP-i vahemälu serveri asukoht
SAP-i vahemälu Server pole juurdepääsu (vahemällu) dokumentide kohalikult serveripõhised komponendi. SAP-i vahemälu serveri vahemälu SAP-i sisu serveri dokumendid. See on optimeerida võrguliiklust, kui dokumendid on rohkem kui üks kord tuuakse erinevatest asukohtadest. Üldine reegel on, et SAP-i vahemälu Server peab olema füüsilise lähedale kliendi serveriga ühenduse SAP-i vahemälu. 

Siin on kaks võimalust:

1. **Kliendi on kirjutamata SAP-i süsteemis** Kui juurdepääsu SAP-i sisu Server on konfigureeritud taustväärtus SAP-i süsteemis, on süsteemi SAP-i klient. Kui nii SAP-i süsteem ja SAP-i serveri on juurutatud Azure piirkonna – sama Azure andmekeskuses – oleksid need üksteisele lähedal. Järelikult ei ole vaja on spetsiaalne SAP-i vahemälu Server. SAP-i Kasutajaliidese kliendid (SAP-i GUI või veebibrauseris) juurdepääsu SAP-i süsteemis otse ja SAP-i sisu Server toob dokumentide SAP-i süsteem.
1. **Kliendi on ka kohapealse veebibrauseris** SAP-i serveri saate konfigureerida pääsevad juurde otse veebibrauseris. Sel juhul töötab – kohapealsete veebibrauseris on SAP-i sisu serveri klient. Kohapealse andmekeskuse ja Azure andmekeskuse paigutatakse eri asukohtades olevate inimestega füüsilise (ideaalvariandis mitte üksteisele lähedal). Teie kohapealse andmekeskuse on ühendatud Azure'i Azure-saidilt VPN- või ExpressRoute kaudu. Kuigi mõlema variandi pakuvad turvalist VPN võrguühenduse Azure, ei paku-saidilt võrguühendus võrgu läbilaskevõime ja latentsusaeg SLA asutusesisese andmekeskuse ja Azure andmekeskuse vahelise. Dokumentidele juurdepääsu kiirendamiseks saate teha ühte järgmistest.
    1. SAP-i vahemälu Server kohapealse installida, Sule asutusesisese veebilehitseja (valik joonisel [this][dbms-guide-900-sap-cache-server-on-premises])
    1. Azure'i ExpressRoute, mis pakub kiire ja latentsusajaga sihtotstarbeline võrgu asutusesisese andmekeskuse ja Azure andmekeskuse vahelise ühenduse konfigureerimine.
 
![SAP-i vahemälu Server kohapealse installimiseks suvandi valik][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Varundamine ja taastamine
Kui konfigureerite SAP-i serveri andmebaasis SAP-i MaxDB failide talletamiseks, varundus ja taaste protseduuri ja jõudluse peaksite arvesse võtma on juba kirjeldatud SAP-i MaxDB Peatükk [varundus ja taaste] [dbms-juhend-8.4.2] ja [jõudluse kaalutluste kohta varundus ja taaste] Peatükk [dbms-juhend-8.4.3]. 

Kui konfigureerite SAP-i serveri failisüsteemi failide talletamiseks, üheks võimaluseks on käivitada käsitsi varundamise taastamine struktuuri terve faili asukohaks dokumendid. Sarnaselt SAP-i MaxDB varundus ja taaste, on soovitatav on spetsiaalne kettadraivil varukoopia eesmärgil. 

#### <a name="other"></a>Muud
Muud SAP-i serveri teatud sätted on läbipaistvaid Azure vms ja on kirjeldatud eri dokumentide ja SAP-i märkmed.

* <https://Service.SAP.com/contentserver> 
* SAP-i Märkus [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>IBM DB2 üksikasjad LUW Windowsi jaoks
Microsoft Azure, saate hõlpsasti olemasoleva SAP-i rakenduse töötavate IBM DB2 Linux, UNIX ja Windows (LUW) Azure'i virtuaalmasinates migreerida. SAP-i IBM DB2 LUW kohta, kus arendajad ja administraatorid saate siiski kasutada sama arendamise ja haldamise tööriistad, mis on saadaval kohapealse.
Üldist teavet SAP Business Suite töötavate IBM DB2 LUW leiate rakenduses on SAP-i ühenduse võrgu (SCN) juures <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Lisateabe saamiseks ja SAP-i jaoks LUW Azure DB2 värskendusi, vt SAP-i Märkus [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linuxi, UNIX ja Windowsi versiooni tugi
SAP-i IBM DB2 LUW Microsoft Azure virtuaalse masina Services kohta on toetatud DB2 versioonis 10.5.

Lisateavet toetatud SAP-i tooted ja Azure VM tüüpi vaadake SAP-i Märkus [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX ja Windows konfigureerimise juhised SAP-i installide Azure VM

#### <a name="storage-configuration"></a>Salvestusruumi konfigureerimine
Kõigi andmebaasi faile hoitakse põhjal VHD ketast NTFS failisüsteemis. Nende VHDs on paigaldatud Azure VM ja asuvad Azure lehe bloobimälu (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Mis tahes tüüpi võrgu kaudu või remote ühtlasi nagu järgmisi teenuseid Azure fail **pole** toetatud andmebaasifailide on: 

* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-files.aspx>
 
Kui kasutate Azure VHDs lehe Azure'i bloobimälu põhjal, rakendada laused, mis on tehtud selles dokumendis peatüki [struktuuri RDBMS juurutamise] [dbms-juhend-2] ka IBM DB2 LUW andmebaasi saama. 

Nagu varem üldine dokumendi osa, kvootide IOPS läbilaskevõime Azure'i VHDs jaoks on olemas. Täpse kvootide sõltuvad VM kasutatavad. VM tüüpide koos oma loendi leiate [siit][virtual-machines-sizes].

Kui praegune IOPS kvoodi ketta kohta on piisavalt, on võimalik ühe ühe ühendatud Azure'i VHD kõiki andmebaasifaile talletada. 

Jõudluse kaalutlused viidata ka peatükk "Andmete turvalisuse ja jõudluse kasutuse jaoks andmebaasi kataloogide" SAP-i installimise juhendid.

Teise võimalusena saate Windowsi salvestusruumi kaustu (ainult Windows Server 2012 saadaval ja kõrgem) või Windowsi striping Windows 2008 R2 nagu on kirjeldatud [tarkvara RAID] [dbms-juhend-2.2] selle dokumendi loomiseks suur üks loogiline seade üle mitme ühendatud VHD ketast.
Ketast, mis sisaldab teie sapdata ja saptmp kataloogide DB2 salvestusruumi liikumisteid, peate määrama füüsilise ketta sektori suurus on 512 KB. Kui kasutate Windows salvestusruumi kaustu, peate looma salvestusruumi kaustu käsitsi kaudu käsurea liides parameetriga "-LogicalSectorSizeDefault". Üksikasju vt <https://technet.microsoft.com/library/hh848689.aspx>.

#### <a name="backuprestore"></a>Varundus ja taaste
Varundus ja taaste funktsiooni IBM DB2 jaoks LUW toetatakse samamoodi nagu tavalise Windows Serveri opsüsteemid ja Hyper-V.

Peate veenduge, et teil on kehtiv andmebaasi varukoopia strateegia kohas. 

Nagu tühjal metallist juurutuste varundus ja taaste jõudlus sõltub mitu mahud saab lugeda paralleelselt ja läbilaskevõime nende mahtu võib olla. Lisaks kasutavad varukoopia tihendamise CPU tarbimine võib mängida olulised VMs koos ainult kuni 8 CPU teemad. Seetõttu üks täita:

* Vähem soovitud arv VHDs kasutada andmebaasi seadmed, talletamiseks väiksemate üldine jõudlus lugemine
* Väiksemate arv Teemad VM rohkem raske varukoopia tihendamise mõju
* Vähem sihtkohtade (triip kataloogide, VHDs) kirjutamise varukoopia, väiksem on läbilaskevõime

Sihtmärgid kirjutamise suurendamiseks kaks võimalust saab kasutada/kombineeritud vastavalt vajadusele:

* Triipude töötlemine varukoopia target helitugevuse üle mitme ühendatud VHDs IOPS läbilaskevõime musta Triibutatud draivile parandamiseks
* Varukoopia, et kirjutada rohkem kui üks target kataloogi kasutamine

#### <a name="high-availability-and-disaster-recovery"></a>Kõrge-saadavus ja katastroofiabi
Microsoft kobar Server (MSCS) ei toetata.

DB2 kõrge-saadavus avariitaastet (HADR) pole toetatud. Kui virtuaalmasinates HA konfiguratsiooni töötamise nimelahendus, Azure häälestamise ei erine mis tahes kohapealse tehtud häälestus. Ei ole soovitatav kasutada ainult IP eraldusvõime.

Ärge kasutage Azure'i poe Geo-kopeerimine. Lisateabe saamiseks vaadake Peatükk [Microsoft Azure'i Tabelimäluga] [dbms-juhend-2.3] ja [kättesaadavuse ja Õnnetusejärgne taastamine Azure'i VMs] Peatükk [dbms-juhend-3].

#### <a name="other"></a>Muud
Kõik muud üldised nagu Azure-saadavus komplektid või SAP-i jälgimine rakendada selle dokumendi juurutuste IBM DB2 LUW VMs ka esimesed kolm peatükke kirjeldatud. 

Vt ka punkt [üldine SQL Server Azure'i Kokkuvõte SAP-i] [dbms-juhend-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>IBM DB2 üksikasjad LUW Linuxi jaoks
Microsoft Azure, saate hõlpsasti olemasoleva SAP-i rakenduse töötavate IBM DB2 Linux, UNIX ja Windows (LUW) Azure'i virtuaalmasinates migreerida. SAP-i IBM DB2 LUW kohta, kus arendajad ja administraatorid saate siiski kasutada sama arendamise ja haldamise tööriistad, mis on saadaval kohapealse. Üldist teavet SAP Business Suite töötavate IBM DB2 LUW leiate rakenduses on SAP-i ühenduse võrgu (SCN) juures <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Lisateabe saamiseks ja SAP-i jaoks LUW Azure DB2 värskendusi, vt SAP-i Märkus [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linuxi, UNIX ja Windowsi versiooni tugi
SAP-i IBM DB2 LUW Microsoft Azure virtuaalse masina Services kohta on toetatud DB2 versioonis 10.5.

Lisateavet toetatud SAP-i tooted ja Azure VM tüüpi vaadake SAP-i Märkus [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX ja Windows konfigureerimise juhised SAP-i installide Azure VM

#### <a name="storage-configuration"></a>Salvestusruumi konfigureerimine
Kõigi andmebaasi failide peavad olema talletatud failisüsteemis, mis põhineb VHD ketast; Nende VHDs on paigaldatud Azure VM ja asuvad Azure lehe bloobimälu (<https://msdn.microsoft.com/library/azure/ee691964.aspx>).
Mis tahes tüüpi võrgu kaudu või remote ühtlasi nagu järgmisi teenuseid Azure fail **pole** toetatud andmebaasifailide on:

* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-files.aspx>

Kui kasutate Azure VHDs lehe Azure'i bloobimälu põhjal, rakendada laused, mis on tehtud selles dokumendis peatüki [struktuuri RDBMS juurutamise] [dbms-juhend-2] ka IBM DB2 LUW andmebaasi saama.

Nagu varem üldine dokumendi osa, kvootide IOPS läbilaskevõime Azure'i VHDs jaoks on olemas. Täpse kvootide sõltuvad VM kasutatavad. VM tüüpide koos oma loendi leiate [siit][virtual-machines-sizes].

Kui praegune IOPS kvoodi ketta kohta on piisavalt, on võimalik ühe ühe ühendatud Azure'i VHD kõiki andmebaasifaile talletada.

Jõudluse kaalutluste viidata ka peatükk "Andmete turvalisuse ja jõudluse kaalutluste jaoks andmebaasi kataloogide" SAP-i installimise juhendid.

Teise võimalusena saate LVM (loogika helitugevuse Manager) või MDADM mida on kirjeldatud [tarkvara RAID] [dbms-juhend-2.2] selle dokumendi loomiseks üle mitme ühendatud VHD ketast suur üks loogiline seade.
Ketast, mis sisaldab teie sapdata ja saptmp kataloogide DB2 salvestusruumi liikumisteid, peate määrama füüsilise ketta sektori suurus on 512 KB.

#### <a name="backuprestore"></a>Varundus ja taaste
Varundus ja taaste funktsiooni IBM DB2 jaoks LUW toetatakse samamoodi nagu kohapealsetes standard Linux installi.

Peate veenduge, et teil on kehtiv andmebaasi varukoopia strateegia kohas.

Nagu tühjal metallist juurutuste varundus ja taaste jõudlus sõltub mitu mahud saab lugeda paralleelselt ja läbilaskevõime nende mahtu võib olla. Lisaks kasutavad varukoopia tihendamise CPU tarbimine võib mängida olulised VMs koos ainult kuni 8 CPU teemad. Seetõttu üks täita:

* Vähem soovitud arv VHDs kasutada andmebaasi seadmed, talletamiseks väiksemate üldine jõudlus lugemine
* Väiksemate arv Teemad VM rohkem raske varukoopia tihendamise mõju
* Vähem sihtkohtade (triip kataloogide, VHDs) kirjutamise varukoopia, väiksem on läbilaskevõime

Sihtmärgid kirjutamise suurendamiseks kaks võimalust saab kasutada/kombineeritud vastavalt vajadusele:

* Triipude töötlemine varukoopia target helitugevuse üle mitme ühendatud VHDs IOPS läbilaskevõime musta Triibutatud draivile parandamiseks
* Varukoopia, et kirjutada rohkem kui üks target kataloogi kasutamine

#### <a name="high-availability-and-disaster-recovery"></a>Kõrge-saadavus ja katastroofiabi
DB2 kõrge-saadavus avariitaastet (HADR) pole toetatud. Kui virtuaalmasinates HA konfiguratsiooni töötamise nimelahendus, Azure häälestamise ei erine mis tahes kohapealse tehtud häälestus. Ei ole soovitatav kasutada ainult IP eraldusvõime.

Ärge kasutage Azure'i poe Geo-kopeerimine. Lisateabe saamiseks vaadake Peatükk [Microsoft Azure'i Tabelimäluga] [dbms-juhend-2.3] ja [kättesaadavuse ja Õnnetusejärgne taastamine Azure'i VMs] Peatükk [dbms-juhend-3].

#### <a name="other"></a>Muud
Kõik muud üldised nagu Azure-saadavus komplektid või SAP-i jälgimine rakendada selle dokumendi juurutuste IBM DB2 LUW VMs ka esimesed kolm peatükke kirjeldatud.

Vt ka punkt [üldine SQL Server Azure'i Kokkuvõte SAP-i] [dbms-juhend-5.8].