<properties 
    pageTitle="App Service 環境でアプリケーションをスケーリングする方法" 
    description="App Service 環境でのアプリのスケーリング" 
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


# App Service 環境でアプリのスケーリング

大まかに言うと、App Service 環境は基本的に Azure App Service の VNET への個人的なデプロイメントであり、サブスクリプションでのみ管理できます。 App Service 環境は VNET 内に配置されるため、新しいネットワーク機能を提供し、さらに Azure App Service 環境で通常提供されるレベル以上の拡張性を提供します。 詳細については、アプリ サービス Environment(ASE) は、ページを参照する必要があるかどうか [App Service 環境 ][whatisase]します。 App Service 環境を作成または App Service 環境で web アプリの作成に関する詳細な情報を参照してください [[howtocreatease] App Service 環境を作成する方法][howtocreatease] と [App Service 環境 ][createwebappinase]

注意して、web、モバイルのスケール属性または、Azure App Service で API アプリに正常に変更するときは変更されるアプリ サービス Plan(ASP) レベルで。 App Service プランのスケーリングや App Service 環境外部の App Service プランの詳細についてのみ詳細を参照してください [[scalewebapp] Azure App Service で web アプリのスケール][scalewebapp] と [アプリ サービス プランの詳細 ][appserviceplans]します。

App Service 環境でのアプリのスケーリングは、通常アプリのスケーリングに非常に似ています。 Azure App Service では、通常、次の 3 つの項目をスケーリングできます。

- 料金プラン
- (専用インスタンスの) ワーカーのサイズ
- インスタンスの数

ASE では、料金プランを選択または変更する必要はありません。 機能に関しては、既に Premium 料金機能レベルにあります。 App Service 環境においても、共有ワーカーは存在しません。 すべて専用ワーカーです。

ワーカーのサイズに関して、ASE 管理者が各ワーカー プールに使用されるコンピューティング リソースのサイズを割り当てることができます。 つまり、必要に応じて、ワーカー プール 1 に P4 コンピューティング リソースを割り当て、ワーカー プール 2 に P1 コンピューティング リソースを割り当てることができます。 サイズの順に従う必要はありません。 サイズとその料金設定の詳細については、こちらのドキュメントを参照してください。 [Azure アプリ サービス料金 ][appservicepricing]します。 結果として、App Service 環境内の Web アプリおよび App Service プランに対しては、次のスケーリング オプションが残されます。

- ワーカー プールの選択
- インスタンスの数

いずれかの項目を変更すると、App Service プラン、ASE がホストされる場合のような適切な UI を通して行われます。 スケール アップ、ASP、ASP が、ワーカー プールで使用可能なコンピューティング リソースの数を超えることはできません。 より多くのコンピューティング リソースが必要な場合は、ASE 管理者に依頼して、必要な量のコンピューティング リソースをワーカー プールに割り当ててもらう必要があります。 ASE を再構成するについては、ここにある情報を参照: [[howtoconfigurease] App Service 環境を構成する方法][howtoconfigurease]します。 また、ASE の自動スケール機能を利用することで、スケジュールまたはメトリックに基づいて容量を追加することができます。 構成の詳細については、ASE 環境自体の自動スケールを参照してください [App Service 環境 ][aseautoscale]します。

![][1]

### インスタンスの数のスケーリング

App Service 環境で Web アプリを初めて作成する場合は、フォールト トレランスを実現するために少なくとも 2 つのインスタンスまでスケーリングする必要があります。

ASE に十分な容量がある場合、この操作は非常に単純です。 スケールアップするサイトが保持されている App Service プランに移動し、[スケール] を選択します。 UI が開きます。ここでは、ASP のスケールを手動で設定することも、ASP の自動スケール ルールを構成することもできます。 単にアプリの規模を調節するには設定 *** をスケール *** に *** 手動で入力したインスタンスの数 ***します。 あとは、スライダーを目的の数量までドラッグするか、スライダーの横のボックスに目的の値を入力します。

![][2]

ASE の ASP 用の自動スケール ルールは、通常と同じように機能します。 選択することができます *** CPU の割合 *** [ *** して拡大縮小 *** または CPU の割合に基づく、ASP を使用して複雑な規則を自動スケール ルールを作成および *** スケジュールおよびパフォーマンス ルール ***します。 構成に関するより完全な詳細を表示する自動スケール ガイドを使用して、ここで [Azure App Service ][appscale]します。


### ワーカー プールの選択

前述のように、ワーカー プールの選択は、ASP UI からアクセスされます。 ASP では、拡張、およびワーカー プールを選択するのには、ブレードを開きます。 App Service 環境内に構成されているすべてのワーカー プールが表示されます。 構成されているワーカー プールが 1 つの場合は、1 つのプールが表示されます。 ASP が属しているワーカー プールを変更するには、単に App Service のプランを移動する場合、ワーカー プールを選択します。

![][3]

ワーカー プール間で ASP を移動する場合は、ASP のための容量が十分に確保されているか事前に確認することが重要です。 ワーカー プールの一覧には、ワーカー プールの名前だけでなく、そのワーカー プールで使用可能なワーカーの数も表示されます。 App Service プランを格納するための十分なインスタンスがあることを確認します。 移動先のワーカー プールにより多くのコンピューティング リソースが必要な場合は、ASE 管理者に依頼してコンピューティング リソースを追加してもらう必要があります。
> [AZURE.NOTE] ASP を 1 つのワーカー プールから移動する ASP では、アプリの再起動によりされます。 これには、少量のアプリを再起動する際に要する時間に応じて、アプリのダウンタイムの可能性があります。  

## 使用の開始

App Service 環境を使ってみる、を参照してください [方法に App Service 環境の作成 ][howtocreatease]。

Azure App Service プラットフォームの詳細については、次を参照してください。 [Azure App Service ][azureappservice]します。




[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png 
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png 
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png 
[whatisase]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/ 
[scalewebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/ 
[howtocreatease]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/ 
[howtoconfigurease]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/ 
[createwebappinase]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/ 
[appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/ 
[appservicepricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[azureappservice]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[aseautoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/ 
[appscale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/ 

