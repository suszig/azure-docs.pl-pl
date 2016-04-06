<properties
    pageTitle="App Service 環境で Web アプリを作成する"
    description="App Service 環境で Web アプリと App Service プランを作成する方法について説明します。"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="10/26/2015"
    ms.author="ccompy"/>

# App Service 環境で Web アプリを作成する

## 概要

このチュートリアルでは web アプリを作成する方法と、App Service プランので、 [App Service 環境](app-service-app-service-environment-intro.md) (ASE)。 

> [AZURE.NOTE] Web アプリを作成する方法については、App Service 環境では、「必要はありません [.NET web アプリを作成する](web-sites-dotnet-get-started.md) またはその他言語やフレームワークに関連するチュートリアルのいずれかです。

## 前提条件

このチュートリアルは、App Service 環境が作成済みであることを前提としています。 まだを実行していない、表示 [App Service 環境の作成](app-service-web-how-to-create-an-app-service-environment.md)します。 

## Web アプリを作成する

1.  [Azure ポータル](https://portal.azure.com/), 、クリックして **新規 > Web + モバイル > Web アプリ**します。 

    ![][1]

2. サブスクリプションを選択します。  

    サブスクリプションを複数保有している場合、App Service 環境でアプリを作成するには、App Service 環境を作成するときに使用したものと同じサブスクリプションを使用する必要があります。 

3. リソース グループを選択または作成します。

    *リソース グループ* の単位として、関連する Azure リソースを管理することを使用してを確立するときに便利ですが *ロールベースのアクセス制御* のアプリ用 (RBAC) ルール。 詳細については、次を参照してください。 [Azure リソースを管理する][ResourceGroups]です。 

4. App Service プランを選択または作成します。

    *App Service プランの* web アプリの管理対象のセットが生成されます。  価格を選択すると、課金される価格は個々のアプリではなく App Service プランに適用されます。 Web アプリのインスタンスの数を増やすには、App Service プランのインスタンスを増やします。これにより、そのプラン内のすべての Web アプリが影響を受けます。  サイトのスロット、VNET 統合などのいくつかの機能には、プラン内での数量制限があります。  詳細については、次を参照してください [Azure App Service プランの概要。](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    ASE で App Service プランを識別するには、プラン名の下に表示されている場所を確認します。  

    ![][5]

    App Service 環境に既に存在する App Service プランを使用する場合は、そのプランを選択します。 新しい App Service プランを作成する場合は、このチュートリアルの次のセクションを参照してください。 [App Service 環境での App Service プランの作成](#createplan)します。

5. Web アプリの名前を入力し、クリックして **作成**します。 

    Web アプリ名は、Azure App Service 内で一意である必要があります。  つまり、"thisismywebapp" という名前の Web アプリ名を作成する場合は、Azure App Service 内に "thisismywebapp" という名前の Web アプリが他に存在していないことが条件となります。  

    ASE 内にある Web アプリの URL は次のとおりです。

    [*sitename*] です。[*App Service 環境の名前*]. p.azurewebsites.net

    となります。次のアドレスにはなりません。

    [*sitename*]. azurewebsites.net

## <a name="createplan"></a>App Service プランを作成する

App Service 環境に App Service プランを作成する場合、ASE には共有ワーカーがないため、ワーカーの選択肢が異なります。  使用する必要があるワーカーは、管理者によって ASE に割り当てられたワーカーです。  つまり、新しいプランを作成するには、ASE ワーカー プールに既に存在しているすべてのプランのインスタンスの総数よりも多くのワーカーを ASE ワーカー プールに割り当てておく必要があります。  プランを作成するのに十分なワーカーが ASE ワーカー プールにない場合は、ASE の管理者に依頼してワーカーを追加してもらう必要があります。

App Service 環境でホストされる App Service プランに関するもう 1 つの違いは、価格の選択肢がないことです。  App Service 環境を利用する場合、システムによって使用されるコンピューティング リソースについて料金を支払い、その環境内のプランに対する追加料金はありません。  通常、App Service プランを作成する際に、価格プランを選択します。これに基づいて、支払い価格が決定されます。  App Service 環境は、基本的に、コンテンツを作成できるプライベートな場所です。  支払いは環境に対するもので、コンテンツをホストする操作を対象にしたものではありません。

次の手順は、チュートリアルの前のセクションで説明した、Web アプリの作成時に App Service プランを作成する方法を示しています。

1. をクリックして **新規作成** プランの選択の UI で ASE の外部では通常と同様に、計画の名前を指定するとします。

2. 場所の選択コントロールで、使用する ASE を選択します。

    App Service 環境は、実質的にプライベートなデプロイの場所であるため、[場所] に表示されます。 

    ![][2]

    場所の選択コントロールで ASE を選択すると、App Service プランの作成用 UI が更新されます。  場所に ASE システムの名前とそのリージョンが表示され、価格プランの選択コントロールがワーカー プールの選択コントロールに置き換えられます。  

    ![][3]

### ワーカー プールの選択

Azure App Service 内の App Service 環境以外の通常の操作では、3 つのコンピューティング サイズが専用の価格プランで選択できるようになっています。  これと似た方法として、ASE では、ワーカーのプールを 3 つまで定義し、そのワーカー プールで使用するコンピューティング サイズを指定できます。  つまり、ASE のテナントは、App Service プランのサイズのコンピューティング料金プランを選択する代わりと呼ばれるものを選択する、 *ワーカー プール*します。  

ワーカー プールの選択 UI では、そのワーカー プールに使用されるコンピューティング サイズがその名の下に表示されます。  "使用可能" な数は、そのプールで使用できるコンピューティング インスタンスの数を示します。  プール全体ではこの数を超えるインスタンスが実際に含まれている可能性がありますが、この値は単に使用されていないインスタンスの数を表します。  多くのコンピューティング リソースを追加する App Service 環境を参照してくださいを調整する必要がある場合 [App Service 環境を構成する](app-service-web-configure-an-app-service-environment.md)です。

![][4]

この例では、使用できるワーカー プールが 2 つしかありません。 これは、ASE 管理者によってこれらの 2 つのワーカー プールのみにホストが割り当てられているためです。  3 つ目のワーカー プールは、VM が割り当てられているときに表示されます。  

## Web アプリを作成した後

ASE での Web アプリの実行と App Service プランの管理に関して、いくつかの点を考慮する必要があります。  

既に説明したように、ASE の所有者は、システムのサイズに責任を持つため、目的の App Service プランをホストするのに十分な容量があることも保証する必要があります。 利用可能なワーカーがない場合は、App Service プランを作成できません。  これは、Web アプリをスケールアップする場合も同じです。  より多くのインスタンスが必要な場合は、App Service 環境の管理者に依頼してワーカーを追加してもらう必要があります。

Web アプリと App Service プランを作成した後は、これをスケールアップすることをお勧めします。  ASE では、アプリのフォールト トレランスを提供するために、常に App Service プランのインスタンスが 2 つ以上必要です。  ASE での App Service プランのスケーリングは、App Service プラン用 UI を使用して通常行うのと同じです。  スケーリングの詳細については [App Service 環境で web アプリをスケーリングする方法](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/


