<properties
    pageTitle="Vaate SAML tagastatud juurdepääsu juhtimine teenuse (Java)"
    description="Saate teada, kuidas vaadata SAML tagastatud Java rakendused Azure'i juhtelemendi ühendust."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Kuidas vaadata SAML tagastatud Azure'i juurdepääsu juhtimine teenus

Sellest juhendist näitab teile, kuidas vaadata selle aluseks oleva turvalisuse väide Markup Language (SAML) tagastatud rakenduse järgi Azure Access Control teenus (ACS). Juhend põhineb [Kuidas autentida Web kasutajatele Azure'i juurdepääsu juhtimine teenuse abil Eclipse][] teema, pakkudes kood, mis kuvab SAML teavet. Täidetud taotluse sarnanema järgmisega.

![Näide SAML väljund][saml_output]

ACS kohta leiate lisateavet jaotisest [järgmised toimingud](#next_steps) .

> [AZURE.NOTE]
> Azure'i Access Services juhtelemendi filtri on ühenduse tehnoloogia eelvaade. Väljalaske-eelse tarkvara, kui see on ametlikult Microsoft ei toeta.

## <a name="prerequisites"></a>Eeltingimused

Sellest juhendist toimingu lõpuleviimiseks täitke kuidas [autentida Web kasutajatele Azure'i juurdepääsu juhtimine teenuse abil Eclipse][] valimi ja kasutada seda lähtepunktina selles õpetuses.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Koosta tee ja juurutamise komplekti JspWriter teegi lisamine

Lisage **javax.servlet.jsp.JspWriter** klassi Koosta tee ja juurutamise kogu sisaldav dokumenditeek. Kui kasutate Tomcat, on teek **jsp-api.jar**, mis asub Apache **teegi** kausta.

1. Eclipse's Project Exploreri Paremklõpsake **MyACSHelloWorld**, nuppu **Tee koostada**, klõpsake **Konfigureerimine koostada tee**, **teekide** vahekaarti ja seejärel klõpsake nuppu **Lisa välise purgid**.
2. Dialoogiboksis **JAR valiku** liikuge vajalikud JAR, valige see ja seejärel klõpsake nuppu **Ava**.
3. **MyACSHelloWorld atribuutide** dialoogiboksis veel avatud, klõpsake **Juurutamise komplekti**.
4. Klõpsake dialoogiboksis **Web juurutamise komplekti** nuppu **Lisa**.
5. **Jaanuari komplekti** dialoogiboksis käsku **Java koostamine tee kirjed** ja klõpsake nuppu **edasi**.
6. Valige sobiv teek ja klõpsake nuppu **valmis**.
7. Klõpsake nuppu **OK** , et sulgeda **MyACSHelloWorld atribuutide** dialoogiboksis.

## <a name="modify-the-jsp-file-to-display-saml"></a>SAML kuvamiseks JSP faili muutmine

Muutke **index.jsp** kasutamiseks järgmist.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Käivitage rakendus

1. Käivitage rakenduse arvuti emulaator või juurutada Azure, kasutades dokumenteerida kuidas [autentida Web kasutajatele Azure'i juurdepääsu juhtimine teenuse abil Eclipse][]juhiseid.
2. Käivitage brauser ja avage veebirakenduses. Pärast sisselogimist rakenduse, kuvatakse teile SAML teavet, sh identiteedi pakkuja turvalisus kinnitus.

## <a name="next-steps"></a>Järgmised sammud

Veelgi uurimine ACS's funktsioonid ja eksperimenteerida keerukamaid stsenaariumid, lugege teemat [Access juhtelemendi teenuse 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Juurdepääsu juhtimine teenuse 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Kuidas abil Eclipse teenusega Azure Access Web kasutajaid autentida]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 