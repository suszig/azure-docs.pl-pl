<properties 
    pageTitle="App Service 環境の作成方法" 
    description="App Service 環境の作成フローの説明" 
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


# App Service 環境の作成方法

App Service 環境 (ASE) は Azure App Service の Premium サービス オプションであり、マルチテナント スタンプでは使用できない高度な構成機能を提供します。 基本的に、ASE 機能は、顧客の仮想ネットワークに Azure App Service を展開します。 大きい値を読み取る App Service 環境が提供する機能を理解する、 [App Service 環境 ][whatisase] ドキュメントです。

### 概要

ASE を作成するには、次の情報を提供する必要があります。

- ASE の名前
- ASE に使用するサブスクリプション
- resource group
- Azure Virtual Network (VNET) とサブネットの選択
- ASE リソース プールの定義

これらの項目ごとにいくつかの重要な詳細情報があります。
- その ASE で行われたすべてのアプリのサブドメインに、ASE の名前が使用されます。
- ASE で行われたすべてのアプリは、ASE 自体と同じサブスクリプションになります
- ASE を作成するために使用するサブスクリプションへのアクセスがあるない場合は、アプリを作成する、ASE を使用できません。
- ASE をホストするために使用する Vnet は地域クラシック"v1"Vnet である必要があります。
- ASE をホストするために使用するサブネットには他のコンピューティング リソースを含めることはできません。
- 1 つだけの ASE は、サブネットに存在できます。

それぞれの ASE デプロイは、Azure によって管理および保守されるホステッド サービスです。 お客様は、ASE システム ロールをホストするコンピューティング リソースにアクセスすることはできませんが、インスタンスの数量とそのサイズを管理できます。

ASE の作成用 UI にアクセスするには、2 つの方法があります。 Azure Marketplace で検索してご覧 *** App Service 環境 *** または新規を経由して Web + モバイル -> です。

VNET 内に ASE とは別のリソース グループを配置する場合は、まず VNET を別途作成してから、ASE の作成時にそれを選択する必要があります。 また、ASE の作成時に既存の VNET 内にサブネットを作成する場合は、VNET と同じリソース グループ内に ASE が存在する必要があります。

### 簡易作成

ASE の作成操作には、短時間で作成を完了できる一連の既定値が用意されています。 展開の名前を入力するだけで ASE をすばやく作成できます。 この場合、ASE は現在地に最も近いリージョンに次の要素を使用して作成されます。

- 512 個のアドレスを持つ VNET
- 256 個のアドレスとサブネット
- 2 つの P2 コンピューティング リソースが含まれたフロントエンド プール
- ワーカー プール 2 P1 コンピューティング リソース
- IP SSL に使用される 1 つの IP アドレス

これは、ASE の最小サイズです。 フロントエンド プールには P2 以上が必要です。  
必ず、ASE で使用するサブスクリプションを選択してください。 ASE を使用してコンテンツをホストできる唯一のアカウントは、その作成に使用されたサブスクリプションに属している必要があります。

![][1]

ASE 用に指定した名前は、ASE で作成したアプリで使用されます。 たとえば、ASE の名前が appsvcenvdemo の場合、ドメイン名は .*appsvcenvdemo.p.azurewebsites.net* になります。 アプリという名前で作成した場合 *ここで mytestapp* でアドレス指定可能なことになりますし、 *mytestapp.appsvcenvdemo.p.azurewebsites.net*します。 ASE の名前は、空白文字を使用できません。 名前に大文字を使用した場合、ドメイン名はその名前をすべて小文字で表記したバージョンになります。

既定値が役に立つ状況は一定数ありますが、多くの場合はなんらかの調整が必要になります。 この後のセクションでは、ASE 関連の各構成セクションについて説明します。

### Virtual Network

自動的に新しい VNET を作成する簡易作成機能が用意されているほか、既存の VNET を選択することも、VNET を手動で作成することもできます。 既存の VNET (現時点でサポートされているのはクラシック "v1" のみ) が App Service 環境のデプロイをサポートするのに十分な大きさを持つ場合は、これを選択できます。 VNET は、以上の 8 文字以上のアドレスが必要です。

既存の VNET を選択する場合は、使用するサブネットを指定するか、または新しいサブネットを作成する必要があります。 サブネットには、8 個以上のアドレスが必要で、事前に他のリソースが含まれていてはいけません。 既に VM が割り当てられているサブネットを使用しようとすると、ASE の作成が失敗します。

VNET の作成用 UI を使用する場合は、次の項目を指定する必要があります。

- VNET 名
- VNET アドレス範囲 (CIDR 表記)
- Location (場所)

ASE が VNET にデプロイされているため、VNET の場所は ASE の場所と同じです。

VNET を指定または選択した後で、サブネットを作成するか選択する必要があります。 ここでを指定する必要が詳細。
- サブネット名
- サブネット範囲 (CIDR 表記)

CIDR (クラスレス ドメイン間ルーティング) 表記とは、IP アドレスと CIDR 値をスラッシュで区切る表記形式です。 次のように *10.0.0.0/22*します。 CIDR 値は、表記されている IP アドレスのサブネット マスクである先頭からのビット数を示しています。 この概念を簡単に言い換えると、CIDR 値は IP 範囲を指定しています。 この例では、10.0.0.0/22 は 10.0.3.255 に 10.0.0.0 送受信 1024年アドレスの範囲を意味します。 /23 は、512 個のアドレスを意味します。

お客様へ既存の VNET にサブネットを作成する場合、ASE に含まれる、VNET と同じリソース グループ。 ASE を VNET とは別のリソース グループに配置するには、ASE を作成する前に、VNET とサブネットを個別に作成してください。

![][2]


### コンピューティング リソース プール

ASE を作成する際、各リソース プールのリソースの数とサイズを設定できます。 リソース プールのサイズは、ASE の作成時に設定できるほか、後で、手動のスケーリング オプションまたは自動スケール オプションを使用して調整することもできます。

前述のように、フロント エンド サーバーとワーカーの ASE で構成されます。 フロントエンド サーバーはアプリの接続の負荷を処理し、ワーカーはアプリのコードを実行します。 フロントエンド サーバーは、専用のコンピューティング リソース プールで管理されます。 ワーカーは、次の名前を持つ 3 つの別個のコンピューティング リソース プールで管理されます。

- ワーカー プール 1
- ワーカー プール 2
- ワーカー プール 3

でしょうがあるシンプルな web アプリに対する要求の数が多い場合、フロント エンドをスケール アップして、おそらくより少ない作業者がいます。 トラフィック量が少ない一方で CPU またはメモリを多く使用する Web アプリの場合、通常はフロントエンドの数を多くする必要はありませんが、より多くのワーカーやサイズの大きなワーカーが必要になります。

![][3]

コンピューティング リソースのサイズに関係なく、最小フット プリントには 2 つのフロントエンド サーバーと 2 つのワーカーが含まれます。 55 個までの最大合計コンピューティング リソースを使用する ASE を構成することができます。 この 55 個のコンピューティング リソースのうち、ワークロードのホストに使用できるのは 50 個のみです。 その理由は 2 つあります。 フロントエンドのコンピューティング リソースは最小で 2 つです。 これにより、ワーカー プールの割り当てのサポートには最大で 53 個残ります。 フォールト トレランスを提供するには、次のルールに従い、追加のコンピューティング リソースを割り当てる必要があります。

- 各ワーカー プールには 1 つ以上の追加のコンピューティング リソースが必要で、これをワークロードに割り当てることはできません。
- プール内のコンピューティング リソースの数量が特定の値を超えた場合、別のコンピューティング リソースが必要になります。

1 つのワーカー プール内でのフォールト トレランスの要件は、ワーカー プールに割り当てられたリソースの任意の値 X について、

- X が 2 ～ 20 の場合、ワークロードに使用できるコンピューティング リソースの数は X-1 です。
- X が 21 ～ 40 の場合、ワークロードに使用できるコンピューティング リソースの数は X-2 です。
- X が 41 ～ 53 の場合、ワークロードに使用できるコンピューティング リソースの数は X-3 です。

任意のプールに割り当てることのできるコンピューティング リソースの数量を管理できるだけでなく、サイズを制御することもできます。 App Service 環境では、P1 ～ P4 のラベルが付けられた 4 種類のサイズから選択することができます。 これらのサイズとその料金設定に関する詳細な情報がここで参照してください。 [App Service の料金 ][appservicepricing]します。 P1 ～ P3 のコンピューティング リソース サイズは、マルチテナント環境で利用できるものと同様です。 P4 コンピューティング リソースでは、8 つのコアと 14 GB の RAM が提供され、App Service 環境でのみ利用できます。

App Service 環境の価格設定は、割り当てられるコンピューティング リソースを対象としています。 ワークロードをホストしているかどうかに関係なく、App Service 環境に割り当てられているコンピューティング リソースに対する料金を支払うことになります。

既定では、ASE は、IP SSL で利用可能な 1 個の IP アドレスを持ちます。 さらに多くの IP アドレスが必要になることがわかっている場合は、ここで設定を指定するか、または作成後に設定を管理することができます。

### App Service 環境を作成した後

ASE を作成した後は、次の項目を調整できます。

- フロントエンドの数 (最小: 2)
- ワーカーの数 (最小: 2)
- IP SSL の使用可能な IP アドレスの数
- フロントエンドまたはワーカーによって使用されるコンピューティング リソースのサイズ (フロントエンドの最小サイズは P2)

次の項目は変更できません。

- 場所
- [サブスクリプション]
- リソース グループ
- 使用する VNET
- 使用するサブネット

手動のスケーリングでは、管理とは、ここの App Service 環境の監視に関する詳細については、: [[aseconfig] App Service 環境を構成する方法][aseconfig]

自動スケーリングに対する情報については、ここでのガイドがあります。
[App Service 環境 ][aseautoscale]

データベース、ストレージなど、カスタマイズできない追加の依存関係があります。 これらは Azure によって処理され、システムに付属しています。 必要に応じて、システムの小数点以下桁数によっては、Azure によってデータベースが調整をシステムのストレージは、App Service 環境全体の最大 500 GB をサポートします。


## 使用の開始

App Service 環境を使ってみる、を参照してください [[whatisase] App Service 環境の概要][whatisase]。

Azure App Service プラットフォームの詳細については、次を参照してください。 [Azure App Service ][azureappservice]します。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]





[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png 
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png 
[3]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-resources.png 
[whatisase]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/ 
[aseconfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/ 
[appservicepricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[azureappservice]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[aseautoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/ 

