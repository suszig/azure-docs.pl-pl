<properties
    pageTitle="Access Control Service によって返される SAML の表示 (Java)"
    description="Azure でホストされる Java アプリケーションで Access Control Service によって返される SAML を表示する方法について説明します。"
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/12/2015" 
    ms.author="robmcm" />

# How to view SAML returned by the Azure Access Control Service (Azure の Access Control Service によって返される SAML を表示する方法)

このガイドでは、Azure の Access Control Service (ACS) によって基盤として使用され、アプリケーションに返される SAML (Security Assertion Markup Language) を表示する方法について説明します。 このガイドに基づき、 [Azure サービスを使用して Eclipse の Access Control で Web ユーザーを認証する方法][] 、SAML 情報を表示するコードを提供することで、トピックです。 完成したアプリケーションは次のようになります。

![SAML の出力例][saml_output]

ACS の詳細については、次を参照してください。、 [次のステップ](#next_steps) セクションです。

> [AZURE.NOTE]
> Azure Access Control Services Filter は Community Technology Preview 版です。 プレリリース版ソフトウェアとしてマイクロソフトによって正式にサポートされていません。

## 前提条件

このガイドのタスクを完了する」のサンプルを完了 [Azure サービスを使用して Eclipse の Access Control で Web ユーザーを認証する方法][] し、このチュートリアルの開始点として使用します。

## ビルド パスとデプロイ アセンブリへの JspWriter ライブラリの追加

含まれているライブラリを追加、 **javax.servlet.jsp.JspWriter** ビルド パスとデプロイ アセンブリへのクラスです。 Tomcat を使用している場合、ライブラリは **jsp api.jar**, 、Apache にある **lib** フォルダーです。

1. Eclipse の Project Explorer で、右クリック **MyACSHelloWorld**, 、] をクリックして **Build Path**, 、] をクリックして **Configure Build Path**, をクリックして、 **ライブラリ** タブをクリックし、をクリックして **Add External JARs**します。
2.  **JAR Selection** ] ダイアログ ボックスで、必要な JAR に移動し、選択して、順にクリックして **開く**します。
3.  **Properties for MyACSHelloWorld** ダイアログ、まだ開いてクリック **Deployment Assembly**します。
4.  **Web Deployment Assembly** ダイアログ ボックスで、をクリックして **追加**します。
5.  **New Assembly Directive** ダイアログ ボックスで、をクリックして **Java Build Path Entries** ] をクリックし、 **次**します。
6. 適切なライブラリを選択し、クリックして **完了**します。
7. をクリックして **OK** を閉じる、 **Properties for MyACSHelloWorld** ダイアログ。

## SAML を表示するように JSP ファイルを変更する

変更 **index.jsp** 次のコードを使用します。

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

## アプリケーションの実行

1. コンピューター エミュレーターでアプリケーションを実行するか」の手順を使用して Azure にデプロイ [Azure サービスを使用して Eclipse の Access Control で Web ユーザーを認証する方法][]します。
2. ブラウザーを起動し、Web アプリケーションを開きます。 アプリケーションにログオンした後、SAML 情報 (ID プロバイダーから提供されるセキュリティ アサーションなど) が表示されます。

## 次のステップ

さらに ACS の機能を調べたりより高度なシナリオを試してみたりする場合、次を参照してください。 [Access Control Service 2.0][]します。

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How to Authenticate Web Users with Azure Access Control Service Using Eclipse]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 
