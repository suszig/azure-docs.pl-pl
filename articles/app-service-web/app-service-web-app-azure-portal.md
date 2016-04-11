<properties
    pageTitle="Azure ポータル内の移動に関するリファレンス"
    description="App Service Web のユーザー エクスペリエンスについて、管理ポータルと Azure ポータルの違いを説明します"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2015"
    ms.author="jaime-espinosa"/>

# クラス ポータル内の移動に関するリファレンス

Azure の web サイトと呼ばれる [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。 この名前の変更を反映し、Azure ポータルの説明も記載するために、すべてのドキュメントの更新が進められています。 この更新プロセスが完了するまでは、Azure ポータルでの Web Apps の操作ガイドとして、このドキュメントを使用できます。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## クラシック ポータルの今後について

クラシック ポータルでブランド変更が行われる一方で、クラシック ポータルを Azure ポータルに置き換えるプロセスが進められています。 クラシック ポータルが廃止されるので、新規開発の焦点は Azure ポータルに移行しつつあります。 Web Apps の今後の新機能はすべて Azure ポータルで提供されます。 Web Apps が提供する優れた最新機能を活用するために、Azure ポータルを使い始めてください。

## クラシック ポータルと Azure ポータルのレイアウトの違い

クラシック ポータルでは、すべての Azure サービスが左側に一覧表示されています。 クラシック ポータルでの移動はツリー構造に従っており、サービスから各要素へと移動します。 この構造は、独立したコンポーネントを管理する場合には適しています。 しかし、Azure で構築されるアプリケーションは相互に接続されたサービスの集合体であり、このツリー構造は、サービスの集合体を操作するうえで最適な構造ではありません。 

Azure ポータルでは、複数のサービスのコンポーネントを使用して、アプリケーションをエンド ツー エンドで簡単に構築できます。 Azure ポータルとして編成 *ジャーニー*します。 A *旅* 一連の *ブレード*, 、さまざまなコンポーネントのコンテナーです。 たとえば、web アプリがに対する自動スケーリングの設定、 *旅* を扱ういくつかのブレードの次の例に示すよう: **web サイト** ブレード (ブレードのタイトルがまだ更新されていないこと、新しい用語を使用する)、 **設定** ブレードで、および **スケール** ブレードです。 例では、自動スケーリングを設定しているがあるために、CPU 使用率に依存する、 **CPU 使用率の** ブレードです。 内のコンポーネント、 *ブレード* と呼ばれます *パーツ*, 、タイルに似ています。 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## 移動の例: Web アプリを作成する

新しい Web アプリの作成方法は、以前と同じように簡単です。 次の画像は、Web アプリを動作させるために必要な手順の数がそれほど変わっていないことを示すために、クラシック ポータルと Azure ポータルを並べて表示しています。 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Azure ポータルでは、WordPress のような人気のギャラリー アプリケーションなど、最も一般的な種類の Web アプリから選択できます。 利用可能なアプリケーションの一覧については、次を参照してください。、 [Azure Marketplace]します。

Web アプリを作成するときは、クラシック ポータルの場合と同様に、Azure ポータルで URL、App Service プラン、場所を指定します。 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

さらに、Azure ポータルでは、他の一般的な設定も定義できます。 たとえば、 [リソース グループ](../resource-group-overview.md) の表示し、関連する Azure リソースの管理を単純化します。 

## 移動の例: 設定と機能

すべての設定と機能は、今では 1 つのブレードへと合理的にグループ化されており、そこから移動を開始することができます。

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

たとえばをクリックしてカスタム ドメインを作成できます **カスタム ドメインと SSL** で、 **設定** ブレードです。

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

監視のアラートを設定するにはクリックして **要求とエラー** し **[アラートの追加**します。

![](./media/app-service-web-app-azure-portal/Monitoring.png)

診断を有効にする] をクリックして **診断ログ** で、 **設定** ブレードです。

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
アプリケーションの設定を構成するにはクリックして **アプリケーション設定** で、 **設定** ブレードです。 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

ブランド名のほかにも、ポータル内のいくつかの要素が、見つけやすくなるように名前を変更されたり、異なるグループに分けられたりしています。 たとえば、次のアプリケーション設定の対応するページのスクリーン ショットに示します (**構成**) 以前のポータルで。

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## その他のリソース

[Azure クラウド アプリケーション プラットフォーム](../app-service-cloud-app-platform.md)

[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
 

