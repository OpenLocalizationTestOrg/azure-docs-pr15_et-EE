<properties
   pageTitle="Rakenduse lüüsi WebSocket tugi | Microsoft Azure'i"
   description="Sellel lehel antakse ülevaade rakenduse lüüsi WebSocket tugi."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Rakenduse lüüsi WebSocket tugi

Lüüsi rakendus toetab kohalikke WebSocket üle kõik lüüsi suurused. On pole kasutaja konfigureeritav säte valikuliselt lubada või keelata WebSocket tugi. Standardse HTTPListener kasutamise jätkamiseks sisse pordi 80 ja 443 WebSocket liikluse vastuvõtmiseks. Seejärel suunatakse WebSocket liikluse WebSocket lubatud taustväärtus server kasutades vastav kirjutamata rakenduskausta rakenduse lüüsi reeglid. WebSocket protokolli [RFC6455](https://tools.ietf.org/html/rfc6455) -i standardiseeritud võimaldab täielikku kahepoolne suhtlemine serveri ja kliendi pikaajalise TCP-ühenduse kaudu. See funktsioon võimaldab omavahel suhelda suhtlemine veebiserverisse ja klient, mis võib olla kahesuunaline ilma vajaduseta Küsitlused on nõutud HTTP-põhine rakendusi.  WebSocket on madal kohal erinevalt HTTP ja saate uuesti kasutada sama TCP-ühenduse mitme taotluse/vastuse tulemuseks tõhusam kasutamine ressursid. WebSocket Protokollid on loodud töötama üle traditsiooniline HTTP pordid 80 ja 443.

Taustväärtus server peab vastata rakenduse lüüsi sondid, mis on kirjeldatud jaotises [seisundi juures ülevaade](application-gateway-probe-overview.md) . Rakenduse lüüsi seisund sondid on HTTP-või HTTPS ainult, see tähendab, et iga taustväärtus server vastama HTTP sondid rakenduse lüüsi serveri WebSocket liikluse marsruutimiseks.

## <a name="listener-configuration-element"></a>Elemendi kuulajale konfigureerimine

Olemasoleva HTTPListener saab toetamiseks WebSocket. Järgmine on valimi mallifailist HttpListeners elemendi tekstifail. Peate HTTP ja HTTPS kuulajatele toetavad WebSocket ja turvalise WebSocket liikluse. Samuti saate [portaali](application-gateway-create-gateway-portal.md) või [PowerShelli](application-gateway-create-gateway-arm.md) luua kuulajatele rakenduste portaali sisse pordi 80 ja 443 toetamiseks WebSocket liikluse.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting ja marsruutimise reeglite konfigureerimine

BackendAddressPool tuleks kasutada määratleda taustväärtus pool WebSocket lubatud serveritega. BackendHttpSetting tuleks määratleda kirjutamata pordi 80 ja 443. Küpsise vastavalt osaleja ja requestTimeouts atribuudid pole oluline WebSocket liikluse. Ei muutu marsruutimise reeglite nõutav. Marsruutimise reeglite "Basic" jätkuvalt kasutada siduda vastav kuulajale vastavate taustväärtus aadress pool. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>Lubatud WebSocket taustväärtus

Teie taustväärtus peab olema HTTP-või HTTPS web server on konfigureeritud töötab port (tavaliselt 80 ja 443) jaoks WebSocket töötamiseks. Selle nõude sellepärast, et WebSocket protokoll nõuab algse käepigistus peaks olema HTTP versiooniuuenduse WebSocket protokolli päise välju.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Mõne muu põhjuseks on selle rakenduse lüüsi kirjutamata seisundi juures toetab ainult HTTP-või HTTPS-protokolle. Kui taustväärtus server ei vasta HTTP-või HTTPS sondid, võetaks välja kirjutamata rakenduskausta ja mitte taotlusi, sh WebSocket taotlusi, jõuaks selle taustväärtus.

## <a name="next-steps"></a>Järgmised sammud

Pärast õppida WebSocket tugi, minge alustada WebSocket lubatud veebirakenduse [rakenduste portaali loomine](application-gateway-create-gateway.md) .
