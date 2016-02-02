<properties
    pageTitle="Azure のデータ サイエンス用仮想マシン | Microsoft Azure"
    description="データ サイエンス用仮想マシンのセットアップ"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="paulettm" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/23/2015"
    ms.author="mohabib;xibingao;bradsev" />


# Azure のデータ サイエンス用仮想マシン

このメニューは、Cortana Analytics Process (CAP) によって使用されるさまざまなデータ サイエンス環境の設定方法を説明するトピックにリンクしています。

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

クラウド ベースのデータ サイエンス環境の一部として使用するように、いくつかの種類の Azure Virtual Machine をプロビジョニングし、構成できます。 使用する仮想マシンの種類を決定する方法は、Machine Learning によってモデル化するデータの種類と量、さらにクラウド内でのデータの宛先によって異なります。


* この決定を行う際の考慮事項については、次を参照してください。 [Azure Machine Learning のデータ サイエンス環境を計画](machine-learning-data-science-plan-your-environment.md)します。
* 実行時に発生するシナリオをいくつかのカタログには、分析が高度なを参照してください [の Advanced Analytics Process and technology Azure Machine Learning の活用シナリオ](../machine-learning-data-science-plan-sample-scenarios.md)。

ここでは、Azure VM と、SQL サービス付きの Azure VM を IPython の Notebook サーバーとして設定する方法について説明します。 IPython Notebook、Azure ストレージ エクスプローラーおよび AzCopy などのサポート ツールに加えて、データ サイエンス プロジェクトに役立つ他のユーティリティのある Windows 仮想マシンを構成します。 Azure ストレージ エクスプローラーと AzCopy は、たとえば、ローカル コンピューターから Azure Storage にデータをアップロードしたり、Azure Storage からローカル コンピューターにデータをダウンロードしたりする便利な手段を提供します。 次の 2 つの指示について説明します。

* [Azure の仮想マシンを高度な分析用の IPython Notebook サーバーとして設定](machine-learning-data-science-setup-virtual-machine.md) IPython Notebook とその他のツールがデータを格納する SQL 以外の Azure ストレージ形式を使用する場合に、データ サイエンスに使用する Azure の仮想マシンをプロビジョニングする方法を示します。

* [Azure SQL Server 仮想マシンを高度な分析用の IPython Notebook サーバーとして設定](machine-learning-data-science-setup-sql-server-virtual-machine.md) IPython Notebook とその他のツールがデータを格納する SQL データベースを使用する場合に、データ サイエンスに使用する Azure SQL Server 仮想マシンをプロビジョニングする方法を示しています。

プロビジョニングと構成が完了すると、これらの仮想マシンは IPython Notebook サーバーとして使用する準備が整い、Azure Machine Learning や Cortana Analytics Process (CAP) と連携して、必要なデータの探索と処理、その他のタスクを実行できるようになります。 データ サイエンス プロセスにおける次のステップがマップされて、 [CAP ラーニング パス](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) SQL Server か HDInsight にデータを処理してサンプリング Azure Machine Learning でデータから学習するための準備を移動する手順が含まれています。

> [AZURE.NOTE] Azure Virtual Machines の料金は**従量課金制**です。 いるを課金されないバーチャル マシンを使用していない場合、必要が内にある、 **停止 (割り当て解除)** 状態から、 [Azure Classic Portal](http://manage.windowsazure.com/)します。 詳細な手順や仮想マシンの割り当てを解除する方法、を参照してください  [シャット ダウンし、未使用時に仮想マシンの割り当てを解除](machine-learning-data-science-setup-virtual-machine.md#shutdown)。





