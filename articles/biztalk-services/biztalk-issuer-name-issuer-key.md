<properties 
    pageTitle="BizTalk Services における発行者名と発行者キー | Microsoft Azure" 
    description="BizTalk サービスの Service Bus または Access Control (ACS) の発行者名と発行者キーを取得する方法について説明します。 MABS、WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="mandia"/>




# BizTalk サービス: 発行者名および発行者キーに関するページ

Azure BizTalk サービスでは、サービス バス発行者名および発行者キーと Access Control 発行者名および発行者キーが使用されます。 具体的には次の処理が行われます。

タスク | 発行者名および発行者キー
--- | ---
Visual Studio からアプリケーションをデプロイする | Access Control 発行者名および発行者キー
Azure BizTalk サービス ポータルを構成する | Access Control 発行者名および発行者キー
Visual Studio で BizTalk Adapter サービスを使用して LOB リレーを作成する | Service Bus 発行者名および発行者キー

このトピックでは、発行者名と発行者キーを取得する手順を示します。 

## Access Control 発行者名および発行者キー
Access Control 発行者名および発行者キーは次のコンポーネントで使用されます。

- Visual Studio で作成された Azure BizTalk サービス アプリケーション: Visual Studio で BizTalk サービス アプリケーションを Azure にデプロイするには、Access Control 発行者名と発行者キーを入力する必要があります。 
- Azure BizTalk サービス ポータル: BizTalk サービスを作成して BizTalk サービス ポータルを開いたときに、Access Control 発行者名および発行者キーが自動的に登録同じ Access Control の値で展開されます。

### Access Control 発行者名および発行者キーをコピーして貼り付けるには

1. サインイン、 [Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)します。
2. 左側のナビゲーション ウィンドウで [ **BizTalk サービス**します。
3. BizTalk サービスを選択します。 
4. 選択 **接続情報** タスク バーにします。 Access Control 名前空間、既定の発行者 (発行者名)、既定のキー (発行者キー) が一覧に表示され、コピーして貼り付けることができます。  

まとめると次のようになります。  
発行者名 = 既定の発行者名  
発行者キー = 既定のキー


選択することも **ACS 管理ポータルを開く** Access Control の値を取得します。

1. サインイン、 [Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)します。
2. 左側のナビゲーション ウィンドウで [ **BizTalk サービス**します。
3. BizTalk サービスを選択します。
4. 接続情報] ボタンを選択し、選択 **ACS 管理ポータルを開く**します。
5. 下にあるポータルで **サービス設定**, [ **サービス Id**します。 これにより、サービス ID が表示されます。これは Access Control 発行者名の値です。 サービス ID リンクを選択すると、パスワードが表示されます。これは発行者キーの値です。 これらの値はコピーできます。<br/><br/>
たとえば、 **サービス Id**, 、「所有者」を参照してください。 "所有者" は Access Control 発行者名です。 「所有者」リンクをクリックすると表示、 **パスワード**します。 "パスワード" リンクをクリックすると、値が表示されます。 このパスワード値は Access Control 発行者キーです。  

まとめると次のようになります。  
発行者名 = サービス ID の名前  
発行者キー = パスワード値

左側のナビゲーション ウィンドウで選択することも **Active Directory** Access Control の値を取得します。 

> [AZURE.IMPORTANT] 使用して Access Control 名前空間の作成時に **Active Directory**, 、サービス Id は **いない** 自動的に作成されました。 BizTalk サービスをプロビジョニングすると、Access Control 名前空間、"所有者" (発行者名) という名前のサービス ID、パスワード (発行者キー)、および対称キーが自動的に作成されます。<br /> 
[方法: ACS 管理サービスを使用して、サービス Id を構成する](http://go.microsoft.com/fwlink/p/?LinkID=303942) Access Control サービス Id について詳しく説明します。


## Service Bus 発行者名および発行者キー
サービス バス発行者名および発行者キーは BizTalk Adapter サービスで使用されます。 Visual Studio の BizTalk サービス プロジェクトでは、BizTalk Adapter サービスを使用して内部設置型の基幹業務 (LOB) システムに接続します。 接続するには、LOB リレーを作成し、LOB システムの詳細を入力します。 このとき、サービス バス発行者名と発行者キーも入力します。

### Service Bus 発行者名および発行者キーを取得するには

1. サインイン、 [Azure クラシック ポータル](http://go.microsoft.com/fwlink/p/?LinkID=213885)します。
2. 左側のナビゲーション ウィンドウで [ **Service Bus**します。
3. 名前空間を選択します。 タスク バーで [ **接続情報**します。 これを表示、 **既定の発行者** (発行者名) と **既定のキー** (発行者キー)。 これらの値はコピーできます。  

まとめると次のようになります。  
発行者名 = 既定の発行者名  
発行者キー = 既定のキー

## 次へ
Azure BizTalk サービスに関するその他のトピック:

-  [Azure BizTalk サービス SDK のインストール](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [チュートリアル: Azure BizTalk サービス](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Azure BizTalk サービス SDK の操作使用開始方法](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk サービス](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## 関連項目
-  [方法: ACS 管理サービスを使用してサービス Id を構成するには](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk サービス: Developer、Basic、Standard および Premium エディションのチャート](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk サービス: Azure を使用して従来のポータルをプロビジョニングします。](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk サービス: プロビジョニング状態チャート](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk サービス: ダッシュ ボード、モニター、スケール タブ](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk サービス: バックアップと復元](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk サービス: 調整](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 

