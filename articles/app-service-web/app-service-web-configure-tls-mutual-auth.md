<properties 
    pageTitle="Kuidas konfigureerida TLS vastastikune autentimine Web App" 
    description="Saate teada, kuidas konfigureerida oma veebirakenduse kasutada TLS kliendi serdi autentimist." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Kuidas konfigureerida TLS vastastikune autentimine Web App

## <a name="overview"></a>Ülevaade ##
Saate piirata juurdepääsu oma Azure veebirakenduse, võimaldades autentimise seda tüüpi. Üks viis selleks on autentida kliendi serdiga kui kutse on SSL/TLS. See on nn TLS vastastikune autentimine või kliendi sert autentimis- ja selles artiklis täpsustab, kuidas häälestamine oma veebirakenduse kasutama kliendi serdi autentimist.

> **Märkus:** Kui teil on juurdepääs saidile üle HTTP ja HTTPS-ei, mitte saate mis tahes kliendi sert. Nii kui teie rakendus nõuab kliendi sertide teil peaks luba kutsed rakenduse http kaudu.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>Kliendi serdi autentimist Web Appi konfigureerimine ##
Häälestamine oma veebirakenduse nõudma kliendi sertide peate lisada sätte clientCertEnabled saidi veebirakenduse jaoks ja määrake selle väärtuseks true. See säte pole praegu saadaval teenuses haldamise portaali kaudu ja REST API tuleb selleks kasutada.

[ARMClient tööriista](https://github.com/projectkudu/ARMClient) abil saate hõlpsalt käsitöö REST API kõne. Pärast seda, kui logite sisse tööriista peate küsimus järgmine käsk:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
Kõik {} asendamine oma veebirakenduse teave ja luua faili nimega enableclientcert.json koos järgmised JSON sisu:

> {"asukoht": "Minu Web Appi asukoht",   
>   "atribuudid": {}  
>     "clientCertEnabled": tõene}}  

Veenduge, et muuta väärtuse "asukoht", kus asub oma veebirakenduse nt Põhja keskse meile või Lääne USA jne.

> **Märkus:** Kui käivitate ARMClient PowerShelli kaudu, peate Paoklahv (ESC) on @ sümbol JSON-faili tagasi rist ".

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Kliendi sert juurdepääsemise Web Appi ##
Kui kasutate ASP.net-i ja konfigureerida rakenduse kasutamiseks kliendi serdi autentimist, allalaadimisvõimalused serdi **HttpRequest.ClientCertificate** atribuuti. Muude rakenduste virnadena, rakenduse kaudu base64 kodeeritud väärtuse "X-ARR-ClientCert" taotluse päises saab kliendi sert. Rakenduse saate selle väärtuse serdi loomine ja seejärel kasutage rakenduse autentimise ja luba eesmärgil.

## <a name="special-considerations-for-certificate-validation"></a>Serdi valideerimine silmas pidada ##
Kliendi sert, mis saadetakse rakendus ei lähe kaudu mis tahes valideerimine veebirakenduste Azure'i platvormi. Selle serdi valideerimisel on veebirakenduse. Siin on valimi kinnitatakse serdi atribuutide autentimise eesmärgil ASP.net-i kood.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
