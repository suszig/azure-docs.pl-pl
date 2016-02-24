<properties 
    pageTitle="Azure App Service と、それが既存の Azure サービスに与える影響" 
    description="新しい Azure App Service とその機能が Azure の既存のサービスにどのように影響するかを説明します。" 
    authors="yochayk" 
    writer="yochayk" 
    editor="yochayk" 
    manager="nirma" 
    services="app-service" 
    documentationCenter=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2015" 
    ms.author="yochayk"/>


# Azure App Service と既存の Azure サービス

この資料では、既存の Azure サービスへの変更を説明するために複数の Azure サービスの変更の一部として [Azure App Service](http://azure.microsoft.com/services/app-service/), 、新しい統合ソリューションです。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## 概要 

[Azure App Service](http://azure.microsoft.com/services/app-service/) 新しく、一意のクラウド サービスにより、web および任意のプラットフォームおよびデバイス向けのモバイル アプリを作成する開発者です。 App Service は、繰り返しのコーディング機能の効率化、エンタープライズ システムと SaaS システムの統合、ビジネス プロセスの自動化などを実現しながら、安全性、信頼性、拡張性のニーズに応えられるよう設計された、統合型ソリューションです。

App Service では、既存の Azure サービスの [Websites](http://azure.microsoft.com/services/websites/), 、[Mobile Services](http://azure.microsoft.com/services/mobile-services/), と [Biztalk サービス](http://azure.microsoft.com/services/biztalk-services/) 、1 つの複合サービスに、強力な新機能を追加するときにします。  App Service では、次の種類のアプリをホストすることができます。 

-   Web Apps
-   Mobile Apps
-   API Apps
-   Logic Apps

次の表では、既存の Azure サービスが、App Service とその中で利用できるアプリの種類に対して、どのような位置付けになっているかを説明します。

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">既存の Azure サービス</th>
<th align="left", style="width:10%">Azure App Service</th>
<th align="left", style="width:80%">変更箇所</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure Websites</td>
<td align="left">Web Apps</td>
<td align="left"><li>Azure web サイトの場合は、App Service は、Web Apps に web サイトの名前を変更するのに厳密に制限されます。
<p><li>Websites のすべての既存インスタンスが、現在は App Service 内の Web Apps になっています。</p>
<p><li>既存の Web サイトに <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure ポータル</a>からアクセスすることが可能で、すべての既存サイトは <em>Web Apps</em>.</p>
<p><li><em>Web ホスティング プランは、</em> 現在は <em>App Service プラン</em>. この <em>App Service プラン</em> では、Web、Mobile、Logic、API など、App Service のすべての種類のアプリをホストできます。</p>
<p><li>Azure App Service Web Apps は、一般向けに提供されています。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Web Apps の詳細についてはこちら</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure Mobile Services</td>
<td align="left">Mobile Apps</td>
<td align="left"><p><li>Mobile Services はスタンドアロン サービスとして引き続き利用でき、現在も完全なサポートが受けられます。</p>
<p><li>Mobile Apps は App Service での新しいアプリの種類で、Mobile Services のすべての機能を含む、多数の機能を統合しています。 Mobile Apps は現在パブリック プレビューの状態です。</p>
<p><li>Mobile Services から <a href="http://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-backend-migrating-from-mobile-services-preview/">Mobile Apps への移行は簡単です</a>. Mobile Apps はプレビュー状態のため、運用環境のアプリの実行にはまだお勧めできません。</p>
<p><li>App Service の一部としては、Mobile Apps は、内部設置型システムと SaaS システムとの統合など、Mobile Services を超える新しい機能に対し、ステージング スロット、web ジョブ、充実したスケール オプションおよび詳細を取得します。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Mobile Apps の詳細についてはこちら</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API Apps</td>
<td align="left">
<p><li>API Apps は App Service での新しいアプリの種類で、これを利用すると、クラウドで簡単に API の構築や使用ができます。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">API Apps の詳細についてはこちら</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logic Apps</td>
<td align="left">
<p><li>Logic Apps は App Service での新しいアプリの種類で、これを利用すると、ビジネス プロセスが簡単に自動化できます。</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Logic Apps の詳細についてはこちら</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk Services</td>
<td align="left">BizTalk API Apps</td>
<td align="left">
<li><p>BizTalk Services はスタンドアロン サービスとして引き続き利用でき、現在も完全なサポートが受けられます。</p>
<li><p>BizTalk Services のすべての機能は、API Apps として App Service に統合されています。これを利用することで、ユーザーは App Service 内のすべての種類のアプリによる企業アプリケーション統合および B2B 統合のシナリオが実現できるようになります。</p>
<li><p>Logic Apps では、視覚的デザイン機能を使用してワークフローを作成し、ビジネス プロセスを自動化できるようになりました。</p></td>
</tr>
</tbody>
</table>

詳細については、次を参照してください [App Service のドキュメント](http://azure.microsoft.com/documentation/services/app-service/)します。
 
