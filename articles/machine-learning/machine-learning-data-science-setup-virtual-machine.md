<properties
    pageTitle="仮想マシンを IPython Notebook サーバーとしてセットアップする | Microsoft Azure"
    description="高度な分析のために、IPython Server を含むデータ サイエンス環境で使用できるように Azure 仮想マシンをセットアップします。"
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
    ms.date="10/13/2015"
    ms.author="mohabib;xibingao;bradsev" />

# 高度な分析のために Azure 仮想マシンを IPython Notebook サーバーとしてセットアップする

このトピックでは、高度な分析のためにデータ サイエンス環境の一部として使える Azure 仮想マシンをプロビジョニングし、構成する方法について説明します。 IPython Notebook、Azure ストレージ エクスプローラー、AzCopy などのサポート ツールと、高度な分析プロジェクトに役立つ他のユーティリティで Windows 仮想マシンを構成します。 たとえば Azure ストレージ エクスプローラーおよび AzCopy には、ローカル マシンから Azure BLOB ストレージにデータをアップロードしたり、BLOB ストレージからローカル マシンにデータをダウンロードしたりするための便利な機能が備わっています。

## <a name="create-vm"></a>手順 1: 汎用の Azure 仮想マシンを作成する

Azure の仮想マシンが既にあるし、IPython Notebook サーバーをセットアップする場合は、この手順をスキップして」に進みます [手順 2: IPython Notebook 用のエンドポイントを既存のバーチャル マシンに追加](#add-endpoint)します。

Azure で仮想マシンを作成するプロセスを始める前に、プロジェクトのデータ処理に必要なマシンのサイズを決定する必要があります。 小さいマシンは大きいマシンに比べてメモリの量と CPU コア数が少ないですが、より安価でもあります。 マシンの種類と価格については、「<a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtual Machines 価格</a>」ページをご覧ください。

1. ログイン <a href="https://manage.windowsazure.com" target="_blank">Azure Classic Portal</a>, 、] をクリック **新規** 左下隅にします。 ウィンドウがポップアップ表示されます。 選択 **コンピューティング** ]-> [ **仮想マシン** ]-> [ **ギャラリーから**します。

    ![Create workspace][24]

2. 次のいずれかのイメージを選択します。

    * Windows Server 2012 R2 Datacenter
    * Windows Server Essentials エクスペリエンス (Windows Server 2012 R2)

    その後、右下にある右向きの矢印をクリックして、次の構成ページに進みます。

    ![Create workspace][25]

3. 作成する仮想マシンの名前を入力し、マシンで処理するデータのサイズおよびマシンの想定機能 (メモリ サイズと CPU コア数) に基づいてマシンのサイズ (既定: A3) を選択し、マシンのユーザー名とパスワードを入力します。 その後、右向きの矢印をクリックして、次の構成ページに進みます。

    ![Create workspace][26]

4. 選択、 **リージョン/アフィニティ グループ/仮想ネットワーク** を含む、 **ストレージ アカウント** をこの仮想マシンに使用し、そのストレージ アカウントを選択し、計画していること。 下部にエンドポイントを追加、 **エンドポイント**  フィールド ("ここで IPython」) のエンドポイントの名前を入力します。 として任意の文字列を選択できます、 **名前** 、終了位置から任意の 0 から 65536 まで整数を **利用可能な** として、 **パブリック ポート**します。  **プライベート ポート** する **9999**します。 ユーザーにする必要があります **回避** インターネット サービスに既に割り当てられているパブリック ポートを使用します。 <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">インターネット サービス用のポート</a>に関するページに、使用しないようにする必要がある割り当て済みのポートの一覧があります。

    ![Create workspace][27]

5. 仮想マシンのプロビジョニング プロセスを開始するには、チェック マークをクリックします。

    ![Create workspace][28]


仮想マシンのプロビジョニング プロセスが完了するには、15 ～ 25 分かかる場合があります。 仮想マシンを作成すると、このマシンの状態と表示されます **を実行している**します。

![Create workspace][29]

## <a name="add-endpoint"></a>手順 2: IPython Notebook 用のエンドポイントを既存の仮想マシンに追加する

「手順 1」の指示に従って仮想マシンを作成した場合、IPython Notebook 用のエンドポイントは既に作成されており、このステップを省略できます。

仮想マシンが既に存在する場合、下記の「手順 3」でインストールする IPython Notebook 用のエンドポイントを追加する必要があれば、まず Azure クラシック ポータルにログインし、仮想マシンを選択して、IPython Notebook サーバー用のエンドポイントを追加します。 次の図は、IPython Notebook 用のエンドポイントを Windows 仮想マシンに追加した後のポータルのスクリーン ショットを示しています。

![Create workspace][17]

## <a name="run-commands"></a>手順 3: IPython Notebook とその他のサポート ツールをインストールする

仮想マシンの作成後、リモート デスクトップ プロトコル (RDP) を使用して Windows 仮想マシンにログオンします。 手順については、次を参照してください。 [Windows Server を実行している仮想マシンにログオンする方法](../virtual-machines-log-on-windows-server.md)します。 開いている、 **コマンド プロンプト** (**Powershell コマンド ウィンドウではない**) として、 **管理者** し、次のコマンドを実行します。

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

インストールが完了したら、IPython Notebook サーバーが自動的に起動、 *C:\\Users\\\ < ユーザー名 \ > \\Documents\\IPython ノートブック* ディレクトリ。

メッセージが表示されたら、IPython Notebook のパスワードと、マシン管理者のパスワードを入力します。 これにより、IPython Notebook をマシン上のサービスとして実行させることができます。

## <a name="access"></a>手順 4: Web ブラウザーから IPython Notebook にアクセスする
IPython Notebook サーバーにアクセスするには、web を開きますブラウザー、および入力 *https://&#60;virtual マシンの DNS 名 >: & #60; パブリック ポート番号 >* URL テキスト ボックスにします。 ここでは、 *& #60; パブリック ポート番号 >* 、IPython Notebook エンドポイントの追加時に指定したポート番号を指定する必要があります。 

 *& #60; 仮想マシンの DNS 名 >* 従来の Azure ポータルで見つかります。 旧ポータルにログインした後をクリックして **仮想マシン**, 、マシンを作成し、[選択 **ダッシュ ボード**, 、DNS 名は、次のように表示されます。

![Create workspace][19]

示す警告が発生 _この web サイトのセキュリティ証明書に問題がある_ (Internet Explorer) または _、接続がプライベート_ (Chrome) と、次に示すように数値です。 クリックして **(非推奨) このサイトの閲覧を続行する** (Internet Explorer) または **詳細** し **に進むと*DNS 名*> (安全でない) * * 続行するには、(Chrome)。 次に、既に指定したパスワードを入力して IPython Notebook にアクセスします。

Internet Explorer:
![Create workspace][20]

Chrome:
![Create workspace][21]

ディレクトリの IPython Notebook にログオンした後 *DataScienceSamples* ブラウザーに表示されます。 このディレクトリには、ユーザーがデータ サイエンス タスクを実行するうえで役立つ、Microsoft が共有するサンプル IPython Notebook が含まれています。 これらのサンプル IPython Notebook がからチェック アウト [**Github リポジトリ**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) IPython Notebook サーバーのセットアップ プロセス中に仮想マシンにします。 Microsoft はこのリポジトリを保守し、頻繁に更新しています。 ユーザーは Github リポジトリを参照して、最近更新されたサンプル IPython Notebook を入手することができます。
![ワークスペースを作成します。][18]

## <a name="upload"></a>手順 5: 既存の IPython Notebook をローカル マシンから IPython Notebook サーバーにアップロードする

IPython Notebook を使用すると、ユーザーはローカル マシン上の既存の IPython Notebook を仮想マシン上の IPython Notebook サーバーに簡単にアップロードできます。 ユーザーが web ブラウザーで IPython Notebook にログオンした後にをクリックして、 **ディレクトリ** IPython Notebook にアップロードされます。 ローカル マシンからアップロードする IPython Notebook の .ipynb ファイルを次に、選択、 **ファイル エクスプ ローラー**, 、してドラッグし、web ブラウザーの IPython Notebook ディレクトリにドロップします。 クリックして、 **アップロード** .ipynb ファイルを IPython Notebook サーバーにアップロードする、ボタンをクリックします。 その後、他のユーザーは自分の Web ブラウザーからそれを使用できるようになります。

![Create workspace][22]

![Create workspace][23]


##<a name="shutdown"></a>未使用時に仮想マシンをシャットダウンして割り当てを解除する

Azure の仮想マシンの料金は **を使用するのには、のみ支払**します。 いるを課金されないバーチャル マシンを使用していない場合、必要が内にある、 **停止 (割り当て解除)** 未使用時に記述します。

> [AZURE.NOTE] VM の内部から仮想マシンをシャット ダウンする (Windows 電源オプションを使用)、VM が停止しますが、割り当てられたままにします。 課金を続行しないことを確認するには、常に仮想マシンを停止、 [Azure Classic Portal](http://manage.windowsazure.com/)します。 呼び出して、Powershell で VM を停止することも **ShutdownRoleOperation** "postshutdownaction"を"StoppedDeallocated"と同じです。

仮想マシンをシャット ダウンして割り当て解除するには、次のようにします。

1. ログイン、 [Azure Classic Portal](http://manage.windowsazure.com/) 、アカウントを使用します。  

2. 選択 **仮想マシン** 左側のナビゲーション バーからです。

3. 仮想マシンの一覧で、仮想マシンの名前をクリックしに移動、 **ダッシュ ボード** ページです。

4. ページの下部にあるをクリックして **シャット ダウン**します。

![VM シャットダウン][15]

仮想マシンの割り当てが解除されますが、削除はされません。 Azure クラシック ポータルから、いつでも仮想マシンを再起動することができます。

## Azure VM を使用する準備ができました。次のステップは ...

これで、仮想マシンをデータ サイエンス演習で使用する準備ができました。 また、仮想マシンを IPython Notebook サーバーとして使用し、データの探索と処理など、Azure Machine Learning および Cortana Analytics Process (CAP) に関連するタスクを行う準備もできました。

Advanced Analytics Process and Technology における次のステップがマップされて、 [学習ガイド: Azure でのデータ処理を高度な](machine-learning-data-science-advanced-data-processing.md) HDInsight にデータを移動して、Azure Machine Learning でデータから学習するための準備にサンプリングする手順が含まれています。


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
 


