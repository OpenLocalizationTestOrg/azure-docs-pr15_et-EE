<properties
   pageTitle="Kuulake rakenduse kasutamiseks turvalisus piiri keskkonnas | Microsoft Azure'i"
   description="Selle lihtsa veebirakenduse, on DMZ loomise järel on testimiseks liikluse kulgemist stsenaariumid juurutamine"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Valimi rakenduse kasutamiseks turvalisus piiri keskkonnas

[Tagasi lehele Turve piiri head tavad][HOME]

Need PowerShelli skriptide saab käitada kohalikult IIS01 ja AppVM01 serverites installimine ja häälestamine väga lihtne veebirakendus, mis kuvab html-lehele serverist esiosa IIS01 sisuga taustväärtus AppVM01 serverist.

See rakendus on lihtne testimise keskkond ette paljud DMZ näited ja kuidas muutuste lõpp-punktid, NSGs, UDR ja tulemüüri reeglid saate efekti liikluse puhul.

## <a name="firewall-rule-to-allow-icmp"></a>Tulemüüri reegel, et lubada ICMP
Selle lihtsa PowerShelli lause saab kasutada mis tahes Windows VM ICMP (Ping) liikluse lubamiseks. See võimaldab lihtsam katsetamine ja tõrkeotsing, lubades ping-protokolli läbi Windowsi tulemüüri (enamik Linux distros ICMP on vaikimisi sees).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Märkus:** Kui kasutate funktsiooni all skriptide, tulemüüri reegli lisamine on esimene lause.

## <a name="iis01---web-application-installation-script"></a>IIS01 - Web rakenduse installimise skripti
See skript järgmist:

1.  Avage IMCPv4 kohaliku serveri Windowsi tulemüür lihtsam testimiseks (Ping)
2.  Installige IIS-i ja .net Framework v4.5
3.  Mõne ASP.net-i veebileht ja Web.config faili loomine
4.  Faili juurdepääsu hõlbustamiseks rakenduse vaikekausta muutmine
5.  Anonüümsete kasutajate määramine oma administraatori konto ja parool

See PowerShelli skripti peaks töötama kohalikult RDP oli IIS01 sisse.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - faili serveri installimise skripti
See skript häälestab selle lihtsa rakenduse tagasi lõppu. See skript järgmist:

1.  Avage IMCPv4 (Ping) tulemüüris oleks lihtsam testimiseks
2.  Looge uus kaust
3.  Tekstifaili kaugühenduse teel olema juurdepääs ülaltoodud veebilehe loomine
4.  Kataloogi ja faili Anonüümne, kui soovite lubada juurdepääsu õiguste määramine
5.  IE täiustatud turvalisus lubamiseks lihtsam sirvimise selles serveris väljalülitamine 

>[AZURE.IMPORTANT] **Parimaks**: kunagi väljalülitamine IE täiustatud turvalisus tootmise server pluss on üldiselt halb mõte sirvida veebi kaudu tootmise server. Lisaks failikettad anonüümse juurdepääsu avamine on hea mõte, kuid valmis siin lihtne.

See PowerShelli skripti peaks töötama kohalikult RDP oli AppVM01 sisse. PowerShelli on vaja käivitamiseks administraatorina eduka täitmise tagamiseks.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - DNS-i serveri installimise skripti
Ei ole skripti, mis sisaldab selle valimi rakenduse häälestamine DNS-i serveri. Kui katsetamine tulemüüri reeglid, NSG või UDR peab sisaldama DNS-i liikluse, DNS01 server peavad olema käsitsi häälestus. Mõlemad näiteid võrgukonfiguratsioon XML-fail sisaldab DNS01 esmane DNS-i server ja avaliku DNS-i server korraldaja tase 3 varukoopia DNS-i server. Tase 3 DNS-i server oleks kasutatakse kohalikud liikluse ja DNS01 tegeliku DNS-i server ei saa seadistada, pole kohalikud DNS-i tekiks.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
