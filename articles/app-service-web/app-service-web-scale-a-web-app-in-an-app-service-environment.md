<properties 
    pageTitle="App Service 環境内でアプリをスケーリングする方法" 
    description="App Service 環境内でのアプリのスケーリング" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/26/2015" 
    ms.author="ccompy"/>

# App Service 環境内でのアプリのスケーリング #

大まかに言うと、App Service 環境は基本的に Azure App Service の VNET への個人的なデプロイメントであり、サブスクリプションでのみ管理できます。 App Service 環境は VNET 内に配置されるため、新しいネットワーク機能を提供し、さらに Azure App Service 環境で通常提供されるレベル以上の拡張性を提供します。  詳細については、どのようなアプリ サービス Environment(ASE) 周囲必要がある場合は、[App Service 環境は] を参照してください [WhatisASE] です。  App Service 環境を作成または App Service 環境で web アプリの作成に関する詳細な情報は、[App Service 環境を作成する方法] を参照してください [HowtoCreateASE] と [App Service 環境で web アプリを作成する方法] [CreateWebappinASE]

通常、Azure App Service で Web アプリ、モバイル アプリ、または API アプリのスケール属性を変更する場合は、App Service プラン (ASP) のレベルで変更が加えられることに注意してください。  App Service プランのスケーリングや App Service 環境外部の App Service プランの詳細についてのみ詳細は、[Azure App Service で web アプリの拡張] を参照してください [ScaleWebapp] と [App Service プランの詳細] [Appserviceplans] です。

App Service 環境でアプリをスケーリングする方法は、アプリをスケーリングする通常の方法とよく似ています。  Azure App Service では、通常、次の 3 つの項目をスケーリングできます。

- 料金プラン
- (専用インスタンスの) ワーカーのサイズ
- インスタンスの数

ASE では、料金プランを選択または変更する必要はありません。  機能に関しては、既に Premium 料金機能レベルにあります。  App Service 環境においても、共有ワーカーは存在しません。  すべて専用ワーカーです。  

ワーカー サイズについては、ASE 管理者が各ワーカー プールに使用するコンピューティング リソースのサイズを割り当てることができます。  つまり、必要に応じて、ワーカー プール 1 に P4 コンピューティング リソースを割り当て、ワーカー プール 2 に P1 コンピューティング リソースを割り当てることができます。  サイズの順に従う必要はありません。  サイズとその料金設定の詳細については、[Azure App Service の料金] [AppServicePricing] ここにドキュメントを参照してください。  結果として、App Service 環境内の Web アプリおよび App Service プランに対しては、次のスケーリング オプションが残されます。

- ワーカー プールの選択
- インスタンスの数

どちらの項目も、ASE でホストされる App Service プランに対して表示される適切な UI を使用して変更できます。  ASP が属しているワーカー プールで利用可能なコンピューティング リソースの数を超えて ASP をスケールアップすることはできません。  より多くのコンピューティング リソースが必要な場合は、ASE 管理者に依頼して、必要な量のコンピューティング リソースをワーカー プールに割り当ててもらう必要があります。  ASE を再構成するについては、ここにある情報を参照します。 [App Service 環境を構成する方法] [HowtoConfigureASE] です。  また、ASE の自動スケール機能を利用することで、スケジュールまたはメトリックに基づいて容量を追加することができます。  ASE 用の自動スケールの構成の詳細を表示するは、環境自体は、[App Service 環境の自動スケールを構成する方法] を参照してください。 [ASEAutoscale] です。

![][1]

### インスタンスの数のスケーリング ###

App Service 環境で Web アプリを初めて作成する場合は、フォールト トレランスを実現するために少なくとも 2 つのインスタンスまでスケーリングする必要があります。   

ASE に十分な容量がある場合、この操作は非常に単純です。  スケールアップするサイトが保持されている App Service プランに移動し、[スケール] を選択します。  UI が開きます。ここでは、ASP のスケールを手動で設定することも、ASP の自動スケール ルールを構成することもできます。  単にアプリの規模を調節するには設定 ***して拡大縮小*** に ***インスタンス数は手動で入力した***します。  あとは、スライダーを目的の数量までドラッグするか、スライダーの横のボックスに目的の値を入力します。  

![][2] 

ASE の ASP 用の自動スケール ルールは、通常と同じように機能します。  選択することができます ***CPU 使用率の*** [ ***して拡大縮小*** または CPU の割合に基づく、ASP を使用して複雑な規則を自動スケール ルールを作成および ***スケジュールおよびパフォーマンス ルール***します。  自動スケールを構成のより完全な詳細を表示するには、ガイドをここで [Azure App Service でのアプリをスケール] を使用して [AppScale] です。 


### ワーカー プールの選択 ###

前述のように、ワーカー プールを選択するには、ASP の UI を使用します。  スケーリングする ASP 用のブレードを開き、[ワーカー プール] を選択します。  App Service 環境内に構成されているすべてのワーカー プールが表示されます。  構成されているワーカー プールが 1 つの場合は、1 つのプールが表示されます。  ASP が属しているワーカー プールを変更するには、App Service プランの移動先のワーカー プールを選択します。  

![][3]

ワーカー プール間で ASP を移動する場合は、ASP のための容量が十分に確保されているか事前に確認することが重要です。  ワーカー プールの一覧には、ワーカー プールの名前だけでなく、そのワーカー プールで使用可能なワーカーの数も表示されます。  App Service プランを格納するための十分なインスタンスがあることを確認します。  移動先のワーカー プールにより多くのコンピューティング リソースが必要な場合は、ASE 管理者に依頼してコンピューティング リソースを追加してもらう必要があります。  

> [AZURE.NOTE] ASP を 1 つのワーカー プールから移動する ASP では、アプリの再起動によりされます。  そのため、アプリが再起動に要する時間に応じて、アプリで短時間のダウンタイムが発生する可能性があります。  

## 使用の開始

App Service 環境の使用開始 [An App Service 環境の作成方法] [HowtoCreateASE] を参照してください。

Azure App Service プラットフォームの詳細については、[Azure App Service] [AzureAppService] を参照してください。

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/

