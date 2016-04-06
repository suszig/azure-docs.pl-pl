<properties 
    pageTitle="Machine Learning ワークスペースの作成 | Microsoft Azure" 
    description="Azure Machine Learning Studio のワークスペースの作成方法" 
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
    ms.date="10/13/2015" 
    ms.author="garye;bradsev"/>


# Azure Machine Learning ワークスペースの作成 

このメニューは、Cortana Analytics Process (CAP) によって使用されるさまざまなデータ サイエンス環境の設定方法を説明するトピックにリンクしています。

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Azure Machine Learning Studio を使用するには、Machine Learning ワークスペースが必要です。 このワークスペースには、実験を管理および公開するのに必要なツールが用意されています。 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## ワークスペースを作成するには

1. Microsoft Azure アカウントにサインインします。
2. Microsoft Azure サービス パネルでクリックして **MACHINE LEARNING**します。

    ![Machine Learning サービス][1]

3. クリックして **+ 新規** ウィンドウの下部にあります。
4. をクリックして **データ サービス**, 、し **MACHINE LEARNING**, 、し **簡易作成**します。

    ![新しいワークスペースの簡易作成][3]

5. 入力、 **ワークスペース名** にワークスペースを指定し、 **ワークスペースの所有者**します。 ワークスペースの所有者は、有効な Microsoft アカウント (name@outlook.com など) である必要があります。

    > [AZURE.NOTE] その後、他のユーザーをワークスペースに招待しての作業中の実験を共有できます。 これを行う Machine Learning Studio での **設定** ページです。 必要な情報は各ユーザーの Microsoft アカウントまたは組織アカウントだけです。

6. Azure を指定 **場所**, 、既存の Azure を入力 **ストレージ アカウント** または選択 **新しいストレージ アカウントの作成** 新規に作成します。
7. クリックして **ML ワークスペースを作成する**です。

一覧に表示されます Machine Learning ワークスペースを作成した後、 **機械学習** ページです。

ワークスペースの管理方法の詳細については、次を参照してください。 [Manage an Azure Machine Learning workspace]します。
ワークスペースの作成の問題が発生した場合は、次を参照してください。 [Troubleshooting guide: Create and connect to an Machine Learning workspace]します。

[Manage an Azure Machine Learning workspace]: machine-learning-manage-workspace.md
[Troubleshooting guide: Create and connect to an Machine Learning workspace]: machine-learning-troubleshooting-creating-ml-workspace.md
 
<!-- ![List of Machine Learning workspaces][2] -->

<!--Anchors-->
[To create a workspace]: #createworkspace

<!--Image references-->
[1]: media/machine-learning-create-workspace/cw1.png
[2]: media/machine-learning-create-workspace/cw2.png
[3]: media/machine-learning-create-workspace/cw3.png



<!--Link references-->
 


