<properties
    pageTitle="PowerApps Enterprise への SQL Server API の追加 | Microsoft Azure"
    description="組織の App Service 環境で新しい SQL Server API を作成または構成し、オンプレミスのデータへの接続を追加します"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="linhtranms"
    manager="dwrede"
    editor=""/>


<tags
   ms.service="powerapps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="11/25/2015"
   ms.author="litran"/>


# 組織の App Service 環境での新しい SQL Server API の作成

## Azure ポータルでの API の作成

1.  [Azure ポータル](https://portal.azure.com/), 、仕事用アカウントでサインインします。 たとえばでサインイン *yourUserName*@*yourcompany ' と*. com です。 これにより、会社のサブスクリプションに自動的にサインインされます。 
2. 選択 **参照** タスク バーで。  
![][14]  
3. 一覧で、PowerApps を検索または入力するスクロールできる *powerapps*:  
![][15]  
4.  **PowerApps**, [ **管理 Api**します。
5.  **管理 Api**, [ **追加** 新しい API を追加します。
6. わかりやすい名前を入力 **名前** した API 向けです。 たとえば、デモでは、SQL Server API を追加する、名前を付けます。 *SQLServerDemo*します。      
7.  **ソース**, [ **利用可能な Api** 構築済みの Api を選択して [ **SQL Server**します。 
8. 選択 **OK** の手順を完了します。

完了すると、App Service 環境に新しい SQL Server API が追加されます。

## オンプレミスの SQL Server に対する接続の構成

オンプレミスの SQL Server に接続できます。 このハイブリッド接続を確立するには、Azure に既に存在する次のようなハイブリッド ネットワーキング ソリューションを利用します。

- [ExpressRoute](../expressroute-introduction.md)
- [サイト間 VPN](../vpn-gateway-create-site-to-site-rm-powershell.md)
- [ポイント対サイト接続](../vpn-gateway-point-to-site-create.md)  

    > [AZURE.NOTE]  すべての app service 環境は、関連付けられている仮想ネットワークを持ちます。 この仮想ネットワークに対してこのネットワーク接続を確立できます。  
- [ハイブリッド接続](../web-sites-hybrid-connection-get-started.md)  

    > [AZURE.NOTE]  App service 環境内での登録されているすべての API は、対応する web アプリを持ちます。 他の Web アプリの場合と同様に、この Web アプリからのハイブリッド接続を確立できます。
    
次の例では、ハイブリッド接続を作成する方法を示します。  

1. 作成した SQL Server API を選択し、リソース グループを選択します。 この例で呼ばれる API を選択して *sqlconnectordemo*, を選択し、 *DedicatedAses* リソース グループ。  
![リソース グループ](./media/powerapps-create-api-sqlserver/sqlapi.png)

2.  選択、 **リソース** タイルし、SQL サーバーの API と同じ名前の web アプリを選択します。 この例では次のように選択します *sqlconnectordemo*:。  
![Sql Web アプリ](./media/powerapps-create-api-sqlserver/sqlwebapp.png)

3.   **設定**, [ **ネットワーク**します。 選択 **ハイブリッド接続エンドポイントを構成する**, をたどります [次の手順](../web-sites-hybrid-connection-get-started.md) ハイブリッド接続を作成します。  
![ネットワーク接続](./media/powerapps-create-api-sqlserver/network.png)

ハイブリッド接続を作成して接続すると、オンプレミスのサーバーに接続できるようになります。 次に、データへの接続を作成し、ユーザーがアクセスできるようにします。  
![ハイブリッド接続](./media/powerapps-create-api-sqlserver/hybridconn.png)

## SQL Server API への接続の作成

1. Azure ポータルで PowerApps を開き、選択 **管理 Api**します。 構成済みの Api の一覧が表示されます。  
  ![](./media/powerapps-create-api-sqlserver/apilist.png)

2. 目的の API を選択します。 この例では次のように選択します。 **SQLServerDemo**, 、を選択して **接続**します。 

3. [接続] で選択 **接続を追加する**:  
![](./media/powerapps-create-api-sqlserver/addconnection.png)

4. 接続の名前と、接続文字列を入力します。 接続文字列を入力するには、接続しているサービスに関するいくつかの特定のプロパティを知っている必要があります。 たとえば、オンプレミスの SQL Server に正常に接続するには、ユーザー名、パスワード、その他のプロパティが必要です。 

5. 選択 **追加** して変更を保存します。

## まとめと次のステップ
このトピックでは、オンプレミスの SQL Server に接続するための SQL Server API を追加しました。 次に、この API をユーザーのアプリケーションに追加できるように、ユーザーに API へのアクセスを許可します。 

[接続を追加し、ユーザーにアクセスを許可する](powerapps-manage-api-connection-user-access.md)


[14]: ./media/powerapps-create-api-sqlserver/browseall.png
[15]: ./media/powerapps-create-api-sqlserver/allresources.png
