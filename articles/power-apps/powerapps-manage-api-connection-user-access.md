<properties
    pageTitle="新しい API の追加または作成と PowerApps におけるユーザーへのアクセス許可の付与 | Microsoft Azure"
    description="Azure ポータルでの新しい API、接続、または接続プロファイルの追加、作成、構成とユーザーへのアクセス許可権限の付与"
    services=""
    suite="powerapps"
    documentationCenter="" 
    authors="MandiOhlinger"
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


# 新しい API の追加、接続の追加、ユーザーへのアクセス許可

Api 内に存在、 [app service 環境](powerapps-get-started-azure-portal.md)します。 API が作成されるのは PowerApps 用の利用可能な API から、App Service 環境でホストされる API アプリから、または Swagger 2.0 からです。 PowerApps に容易に追加できる事前構築の API はたくさんあります。 自分の API を JSON 形式または Swagger 2.0 にてアップロードすることもできます。 

こちらのトピック:

- API を PowerApps に追加し、社内のユーザーに API 使用のためのアクセス許可を付与する手順 (API のプロパティ変更を含む) を説明します。
- API に接続を追加し、社内のユーザーに接続使用のためのアクセス許可を付与する手順を説明します。


#### 開始のための前提条件

- 有効にする [、Azure サブスクリプションに PowerApps](powerapps-get-started-azure-portal.md)します。
- 作成、 [App Service 環境](powerapps-get-started-azure-portal.md)します。
- 次の方法のいずれかを使用して API を作成する。  
    - 作成、 [マネージ API を Microsoft IT のマネージ API](powerapps-register-from-available-apis.md)します。
    - 内でホストされる API の作成 [App Service 環境内](powerapps-register-api-hosted-in-app-service.md)します。
    - 使用して作成、 [Swagger 2.0 API 定義](powerapps-register-existing-api-from-api-definition.md)します。


## ユーザーへの API アクセス許可
API を作成して、App Service 環境に追加したら、社内のユーザーに使用許可を与える手順に入ります。 

1. PowerApps で [ **管理 Api**, 、API を選択します。 たとえば、作成した場合、 *MS Power BI* API を選択してブレードを開きます。 選択 **API へのユーザー アクセス**:  
![][1]  

2. 選択 **追加** ユーザーを追加し、権限を選択します。 終了したら、選択 **追加** して変更を保存します。 ユーザーまたはグループ内の増加の数、 **API へのユーザー アクセス** ウィンドウです。


## API への新しい接続の追加
次の手順は API に対する "接続" の作成です。これは接続文字列のようなものです。 これにより、API が正常に「バックエンド」システムに接続可能になります。 PowerApps Enterprise パブリック プレビューについては、SQL Server の接続のみが追加および構成可能です。 これでさらに追加されました。 

- [SQL Server の接続の作成](powerapps-create-api-sqlserver.md)

## ユーザーへの接続に対するランタイム アクセスの付与
社内ユーザーに接続を使用するアクセス許可を行います。

1. API を開き、選択 **接続**, 、特定の接続を選択します。 これにより、接続名が上部に表示されている新しいブレードが開きます。 
2. この新しいブレードで、選択 **接続ユーザーのアクセスを**します。  次の例では、 **ハイブリッド トンネル** 接続が選択されています。 新しいブレードが開き、これは、選択した **接続ユーザーのアクセスを**:  
![][2]
  
3.  **接続ユーザーのアクセスを**, を選択 **追加**, に付与するアクセス許可を選択します。  
![][3]
  
4. ユーザーまたはグループを追加します。 選択 **追加** して変更を保存します。

ユーザーが API とその接続に対してアクセス許可を得たので、ユーザーはこれらの API を PowerApps で作成したアプリに追加できるようになりました。 具体的には次の処理が行われます。 

- API の下に一覧表示を表示する **利用可能な接続** PowerApps にします。
- [接続を表示する **接続** PowerApps にします。


## API の削除
以前に追加した API を削除することもできます。 PowerApps で、選択、 **管理 Api**, 、API を選択してから選択 **削除**:  
![][4]


## まとめと次のステップ
このトピックで説明した内容は次のとおりです。

- API を追加して、社内ユーザーに使用する権限を付与しました。 これらの手順により、ランタイム アクセスをいつでも管理することが可能です。 たとえば、ユーザー A が退職した場合、Azure ポータルを使用してユーザーのアクセス許可を容易に削除できます。 ユーザー B が入社した場合にも同じシナリオです。
- 接続を追加しました (接続文字列に似ています)。 この手順により、Azure でホストされている API がオンプレミスの SQL Server のようにシステムに接続できるようになります。 また、社内のユーザーに接続を使用できるアクセス許可を付与しました。 
- タスクごとに異なるブレードにて作業しました。 接続を追加するため、API を開いてそのブレードを使用します。 ユーザーのアクセスを許可するには、アクセスを付与する対象に合わせて、API または接続を開きます。 
- App Service 環境で作成した API はいずれも削除が可能です。

次に、実行できます [の管理し、監視、PowerApps](powerapps-manage-monitor-usage.md)します。

[1]: ./media/powerapps-manage-api-connection-user-access/apiuseraccess.png
[2]: ./media/powerapps-manage-api-connection-user-access/connectionuseraccess.png
[3]: ./media/powerapps-manage-api-connection-user-access/selectpermission.png
[4]: ./media/powerapps-manage-api-connection-user-access/deleteapi.png

