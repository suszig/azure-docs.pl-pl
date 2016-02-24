<properties
    pageTitle="Machine Learning ワークスペースの管理 | Microsoft Azure"
    description="Azure Machine Learning ワークスペースへのアクセスを管理し、ML API Web サービスをデプロイおよび管理します"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/15/2015"
    ms.author="garye"/>


# Azure Machine Learning ワークスペースの管理
Azure クラシック ポータルを使用すると、Machine Learning ワークスペースで次の操作を管理できます。

- ワークスペースの使用方法を監視する
- アクセスを許可または拒否するようにワークスペースを構成する
- ワークスペースで作成した Web サービスを管理する
- ワークスペースを削除する

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

また、[ダッシュボード] タブには、ワークスペースの使用状況の概要とワークスペース情報の要約が表示されます。  

> [AZURE.TIP] Azure Machine Learning Studio での **WEB サービス** タブ、追加できますが、更新、または Machine Learning web サービスを削除します。

ワークスペースを管理するには

1.  Azure サブスクリプションに関連付けられているアカウントを使用して、Microsoft Azure アカウントにサインインします。
2.   [Azure Classic Portal](https://manage.windowsazure.com/), 、Microsoft Azure サービス パネルでクリックして **MACHINE LEARNING**します。
3.  管理するワークスペースをクリックします。

ワークスペースのページには次の 3 つのタブがあります。

- **ダッシュ ボード** -ワークスペースの使用状況の表示と情報を
- **構成** -ワークスペースへのアクセスを管理することができます
- **WEB サービス** -このワークスペースから公開されている web サービスを管理することができます


## ワークスペースの使用方法を監視するには

クリックして、 **ダッシュ ボード** ] タブをクリックします。

ダッシュボードには、ワークスペースの全体的な使用状況と、ワークスペース情報の概要が表示されます。

-  **コンピューティング** グラフは、ワークスペースで使用されるコンピューティング リソースを示します。 ビューを変更して相対値または絶対値を表示したり、グラフに表示される期間を変更したりできます。
- **使用状況の概要** 、ワークスペースで使用されている Azure のストレージを表示します。
- **[概要** ワークスペース情報や役に立つリンクの概要を示します。

> [AZURE.NOTE]  **ML Studio にサインイン** リンクに現在サインインしている Microsoft アカウントを使用して Machine Learning Studio を開きます。 Azure クラシック ポータルへのサインインに使用する Microsoft アカウントでワークスペースを作成する場合は、ワークスペースを開く権限が自動的には付与されません。 ワークスペースを開くには、ワークスペースの所有者として定義された Microsoft アカウントにサインインするか、所有者からワークスペースへの参加の招待を受け取る必要があります。


## ユーザーのアクセスを許可または一時停止するには ##

クリックして、 **構成** ] タブをクリックします。

[構成] タブで次の操作を実行できます。

- [拒否] をクリックして、Machine Learning ワークスペースへのアクセスを中断します。 ユーザーは、Machine Learning Studio でワークスペースを開くことができなくなります。 アクセスを復元するには、[許可] をクリックします。
- 他の Microsoft アカウントを指定して、ワークスペースの所有者を変更します。

Machine Learning Studio でワークスペースにアクセスできる追加のアカウントを管理する] をクリックして **ML Studio にサインイン** で、 **ダッシュ ボード** ] タブ (注を参照については、上記 **ML Studio にサインイン**)。 Machine Learning Studio でワークスペースが開きます。 ここでは、クリックして、 **設定** ] タブをクリックし、 **ユーザー**します。 クリックして **INVITE MORE USERS** 、ワークスペースにユーザーがアクセスできるように、またはユーザーを選択し、をクリックする **削除**します。


## このワークスペースの Web サービスを管理するには

クリックして、 **WEB サービス** ] タブをクリックします。

このワークスペースから発行された Web サービスの一覧が表示されます。
Web サービスを管理するには、一覧で名前をクリックして Web サービス ページを開きます。

Web サービスには、1 つ以上のエンドポイントが定義されている場合があります。

- "既定" のエンドポイントの他に追加のエンドポイントを定義できます。 エンドポイントを追加するには、次のようにクリックします。 **エンドポイントの追加** ページの下部にあります。

- 削除する (「既定」のエンドポイントを削除することはできません) エンドポイントをエンドポイント行の名前以外の任意の場所をクリックし、をクリックして **エンドポイントの削除** ページの下部にあります。 Web サービスからエンドポイントが削除されます。

    > [AZURE.NOTE] エンドポイントを削除すると、アプリケーションは web サービスのエンドポイントに使用されている場合、アプリケーション エラーを受け取ります次回サービスにアクセスしようとします。

Web サービス エンドポイントの名前をクリックして開きます。 使用状況グラフに、Web サービス エンドポイントで使用されているコンピューティングと予測のリソースが表示されます。 ビューを変更して相対値または絶対値を表示したり、グラフに表示される期間を変更したりできます。

このページには、Web サービスの REST API を使用してエンドポイントにアクセスする必要がある情報も表示されます。 詳細については、次を参照してください。 [Azure Machine Learning web サービスを使用する方法について][consume]します。

このページから Azure Marketplace に Web サービスを発行することもできます。 詳細については、次を参照してください。 [Azure Machine Learning Web サービスの発行 Azure Marketplace に][marketplace]します。

クリックして、 **構成** を説明を編集して、web サービスでは、同時接続数を制御または診断トレースを設定] タブをクリックします。

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md

