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

このガイドでは、Azure の Access Control Service (ACS) によって基盤として使用され、アプリケーションに返される SAML (Security Assertion Markup Language) を表示する方法について説明します。 このガイドに基づき、 [アクセス制御サービスを使用した Azure で Web ユーザーを認証する方法版 Eclipse][] 、SAML 情報を表示するコードを提供することで、トピックです。 完成したアプリケーションは次のようになります。

![SAML の出力例][saml_output]

ACS の詳細については、次を参照してください。、 [次のステップ](#next_steps) セクションです。
> [AZURE.NOTE]
> Azure Access Control Services Filter は Community Technology Preview 版です。 プレリリース版ソフトウェアとしてマイクロソフトによって正式にサポートされていません。

## 前提条件

このガイドのタスクを完了する」のサンプルを完了 [アクセス制御サービスを使用した Azure で Web ユーザーを認証する方法版 Eclipse][] し、このチュートリアルの開始点として使用します。

## ビルド パスとデプロイ アセンブリへの JspWriter ライブラリの追加

**javax.servlet.jsp.JspWriter** クラスを格納するライブラリをビルド パスとデプロイ アセンブリに追加します。 Tomcat を使用している場合、ライブラリは Apache の **lib** フォルダー内の **jsp-api.jar** です。

1. Eclipse の Project Explorer で、**MyACSHelloWorld** を右クリックし、**[Build Path]**、**[Configure Build Path]**、**[Libraries]** タブ、**[Add External JARs]** の順にクリックします。
2. **[JAR Selection]** ダイアログ ボックスで、必要な JAR に移動して選択し、**[Open]** をクリックします。
3. **[Properties for MyACSHelloWorld]** ダイアログ ボックスが開いた状態で、**[Deployment Assembly]** をクリックします。
4. **[Web Deployment Assembly]** ダイアログ ボックスで **[Add]** をクリックします。
5. **[New Assembly Directive]** ダイアログ ボックスで、**[Java Build Path Entries]**、**[Next]** の順にクリックします。
6. 該当するライブラリを選択し、**[Finish]** をクリックします。
7. **[OK]** をクリックして **[Properties for MyACSHelloWorld]** ダイアログ ボックスを閉じます。

## SAML を表示するように JSP ファイルを変更する

次のコードを使用するように **index.jsp** を変更します。

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

1. コンピューター エミュレーターでアプリケーションを実行するか」の手順を使用して Azure にデプロイ [アクセス制御サービスを使用した Azure で Web ユーザーを認証する方法版 Eclipse][]します。
2. ブラウザーを起動し、Web アプリケーションを開きます。 アプリケーションにログオンした後、SAML 情報 (ID プロバイダーから提供されるセキュリティ アサーションなど) が表示されます。

## 次のステップ

さらに ACS の機能を調べたりより高度なシナリオを試してみたりする場合、次を参照してください。 [Access Control Service 2.0 の []][]します。


[prerequisites]: #pre 
[modify the jsp file to display saml]: #modify_jsp 
[add the jspwriter library to your build path and deployment assembly]: #add_library 
[run the application]: #run_application 
[next steps]: #next_steps 
[access control service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360 
[how to authenticate web users with azure access control service using eclipse]: ../active-directory-java-authenticate-users-access-control-eclipse 
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png 

