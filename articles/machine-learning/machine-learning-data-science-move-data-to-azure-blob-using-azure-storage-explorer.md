<properties 
    pageTitle="Azure Storage Explorer を使用して Azure BLOB ストレージ間でデータを移動する | Microsoft Azure" 
    description="Azure Storage Explorer を使用して Azure BLOB ストレージ間でデータを移動する" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/23/2015" 
    ms.author="bradsev" />

# Azure Storage Explorer を使用して Azure BLOB ストレージ間でデータを移動する

以下のリンクから、Azure Blob ストレージとの間でデータを移動するために使用するテクノロジについてのガイダンスを参照してください。

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

## はじめに

Azure ストレージ エクスプローラーは、Azure Storage アカウント内のデータを検査および変更するための無料の Windows ベースのツールです。 ダウンロードできます [Azure ストレージ エクスプ ローラー](http://azurestorageexplorer.codeplex.com/)します。 

> [AZURE.NOTE] によって提供されるスクリプトに設定されている VM を使用しているかどうかは [Azure データ サイエンス用仮想マシン](machine-learning-data-science-virtual-machines.md), 、VM で Azure ストレージ エクスプ ローラーが既にインストールされている、します。

> [AZURE.NOTE] Azure blob ストレージに完全な概要についてを参照してください [Azure Blob の基礎](../storage-dotnet-how-to-use-blobs.md) と  [Azure Blob サービス](https://msdn.microsoft.com/library/azure/dd179376.aspx)します。 

## 前提条件

このドキュメントは、Azure サブスクリプション、ストレージ アカウント、そのアカウントに対応するストレージ キーがあることを前提としています。 データのアップロード/ダウンロードを行う前に、Azure Storage のアカウント名とアカウント キーを確認しておく必要があります。 

- Azure サブスクリプションを設定するを参照してください。 [無料評価版の 1 か月](https://azure.microsoft.com/pricing/free-trial/)します。
- ストレージ アカウントを作成する手順について、およびアカウントとキー情報を取得するためには、「 [は Azure ストレージ アカウント](../storage-create-storage-account.md)します。


<a id="explorer"></a>
## Azure ストレージ エクスプローラーの使用 

次の手順では、Azure ストレージ エクスプローラーを使用してデータをアップロード/ダウンロードする方法について説明します。 

1.  Azure ストレージ エクスプローラーの起動 
2.  アクセスするストレージ アカウントが、Azure ストレージ エクスプローラーに追加されていない場合は、[アカウントの追加] ボタンをクリックしてアカウントを追加します。 既に追加されている場合は、「--ストレージ アカウントの選択--」ドロップダウンから、アカウントを選択します。  
![Create workspace][1]
<br>
3. ストレージ アカウント名とストレージ アカウント キーを入力して、[ストレージ アカウントの追加] をクリックします。 複数のストレージ アカウントを追加できます。その場合、各アカウントがタブに表示されます。 このストレージ アカウント下のコンテナーは、左側のパネルに表示されます。 コンテナーを選択すると、右側のパネルにコンテナー内の BLOB が表示されます。  
![Create workspace][2]
<br>
![Create workspace][3]
<br>
4. データをアップロードするには、[アップロード] ボタンをクリックします。 アップロードする 1 つまたは複数のファイルをファイル システムから選択し、[開く] をクリックしてファイルのアップロードを開始します。
5. 対応するコンテナー内の BLOB を選択し、[ダウンロード] ボタンをクリックして、データをダウンロードします。

<!-- Images -->

[1]: ./media/machine-learning-data-science-move-azure-blob/data-science-process-uploading-data-to-blob-storage-img1.png
[2]: ./media/machine-learning-data-science-move-azure-blob/data-science-process-uploading-data-to-blob-storage-img2.png
[3]: ./media/machine-learning-data-science-move-azure-blob/data-science-process-uploading-data-to-blob-storage-img3.png

