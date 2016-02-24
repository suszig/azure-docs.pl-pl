<properties 
   pageTitle="AS2 コネクタ メッセージのアーカイブ | Microsoft Azure App Service" 
   description="Azure App Service に AS2 コネクタ メッセージをアーカイブまたは保存する方法" 
   services="app-service\logic" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="11/30/2015"
   ms.author="rajram"/>


# AS2 コネクタ メッセージのアーカイブの概要

**AS2 コネクタ** はメッセージをアーカイブする機能を公開します。 内のメッセージを格納するアーカイブ、 **Azure Blob コンテナー** パッケージの設定の一部であります。 

アーカイブ機能は、次の 2 つの時点で、メッセージと受信確認 (MDN) の両方に公開されます。

1. **受信/デコード トリガー**: メッセージは、API アプリ インスタンスが受け取るとすぐにアーカイブされます 
2. **エンコード/送信アクション**: エンコードされたメッセージのアーカイブ、すべての処理が完了しだけで、パートナーに送信される前に 

## 方法: アーカイブされたメッセージの URL を取得する

AS2 コネクタの API アプリ インスタンスを参照して [追跡] をクリックします。 フィルター パラメーターを使用して追跡情報を絞り込みます。 目的のメッセージがビューに表示されたら、クリックして詳細ビューを確認します。 この詳細ビューには、アーカイブされたメッセージの URL が表示されます。  
![][1]  

## 方法: アーカイブされたメッセージを取得する

前に取得した URL を使用して、アーカイブされたメッセージを Azure BLOB ストレージから取得します。


<!--Image references-->
[1]: ./media/app-service-logic-archive-as2-messages/Tracking.jpg
 

