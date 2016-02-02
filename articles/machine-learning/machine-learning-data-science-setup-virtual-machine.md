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

## <a name="create-vm"></a>手順 1: 汎用の Azure 仮想マシンを作成します。

Azure の仮想マシンが既にあるし、IPython Notebook サーバーをセットアップする場合は、この手順をスキップして」に進みます [手順 2: IPython Notebook 用のエンドポイントを既存のバーチャル マシンに追加](#add-endpoint)します。

Azure で仮想マシンを作成するプロセスを始める前に、プロジェクトのデータ処理に必要なマシンのサイズを決定する必要があります。 小さいマシンは大きいマシンに比べてメモリの量と CPU コア数が少ないですが、より安価でもあります。 マシンの種類と価格については、次を参照してください、 <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtual Machines 料金 </a> ページ。

1. ログイン <a href="https://manage.windowsazure.com" target="_blank">Azure Classic Portal</a>, 、] をクリック **新規** 左下隅にします。 ウィンドウがポップアップ表示されます。 **[Compute]** -> **[仮想マシン]** -> **[ギャラリーから]** の順にクリックします。

    ![Create workspace][24]

2. 次のいずれかのイメージを選択します。

    * Windows Server 2012 R2 Datacenter
    * Windows Server Essentials エクスペリエンス (Windows Server 2012 R2)

    その後、右下にある右向きの矢印をクリックして、次の構成ページに進みます。

    ![ワークスペースの作成][25]

3. 作成する仮想マシンの名前を入力し、マシンで処理するデータのサイズおよびマシンの想定機能 (メモリ サイズと CPU コア数) に基づいてマシンのサイズ (既定: A3) を選択し、マシンのユーザー名とパスワードを入力します。 その後、右向きの矢印をクリックして、次の構成ページに進みます。

    ![Create workspace][26]

4. この仮想マシンに使用する予定の **[ストレージ アカウント]** を含む **[リージョン/アフィニティ グループ/仮想ネットワーク]** を選択し、そのストレージ アカウントを選択します。 下部にエンドポイントを追加、 **エンドポイント**  フィールド ("ここで IPython」) のエンドポイントの名前を入力します。 エンドポイントの **[名前]** として任意の文字列を選択し、**[パブリック ポート]** として**使用可能な** 0 から 65536 までの任意の整数を選択できます。 **[プライベート ポート]** は **9999** でなければなりません。 ユーザーは、インターネット サービスに既に割り当てられているパブリック ポートの使用を**避ける**必要があります。 <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">インターネット サービス用のポート</a> が割り当てられているし、避ける必要がありますポートの一覧を示します。

    ![ワークスペースの作成][27]

5. 仮想マシンのプロビジョニング プロセスを開始するには、チェック マークをクリックします。

    ![Create workspace][28]


仮想マシンのプロビジョニング プロセスが完了するには、15 ～ 25 分かかる場合があります。 仮想マシンが作成されると、このマシンの状態が **[実行中]** と表示されます。

![Create workspace][29]

## <a name="add-endpoint"></a>手順 2: IPython Notebook の既存のバーチャル マシンにエンドポイントを追加します。

「手順 1」の指示に従って仮想マシンを作成した場合、IPython Notebook 用のエンドポイントは既に作成されており、このステップを省略できます。

仮想マシンが既に存在する場合、下記の「手順 3」でインストールする IPython Notebook 用のエンドポイントを追加する必要があれば、まず Azure クラシック ポータルにログインし、仮想マシンを選択して、IPython Notebook サーバー用のエンドポイントを追加します。 次の図は、IPython Notebook 用のエンドポイントを Windows 仮想マシンに追加した後のポータルのスクリーン ショットを示しています。

![Create workspace][17]

## <a name="run-commands"></a>手順 3: IPython Notebook とその他のサポート ツールをインストールします。

仮想マシンの作成後、リモート デスクトップ プロトコル (RDP) を使用して Windows 仮想マシンにログオンします。 手順については、次を参照してください。 [Windows Server を実行している仮想マシンにログオンする方法](../virtual-machines-log-on-windows-server.md)します。 **管理者**として (**Powershell コマンド ウィンドウではなく**) **コマンド プロンプト**を開き、次のコマンドを実行します。

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
    
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

インストールが完了したら、IPython Notebook サーバーが自動的に起動、* C:\\Users\\\<user name\>\\Documents\\IPython ノートブック * ディレクトリ。

メッセージが表示されたら、IPython Notebook のパスワードと、マシン管理者のパスワードを入力します。 これにより、IPython Notebook をマシン上のサービスとして実行させることができます。

## <a name="access"></a>手順 4: web ブラウザーから IPython Notebook にアクセスします。

IPython Notebook サーバーにアクセスするには、web を開きますブラウザー、および入力 *https://&#60;virtual マシンの DNS 名 >: & #60; パブリック ポート番号 >* URL テキスト ボックスにします。 ここでは、 *& #60; パブリック ポート番号 >* 、IPython Notebook エンドポイントの追加時に指定したポート番号を指定する必要があります。

*&#60;仮想マシンの DNS 名>* は、Microsoft Azure クラシック ポータルで確認できます。 クラシック ポータルにログインしたら、**[仮想マシン]** をクリックし、作成済みのマシンを選択して **[ダッシュボード]** を選択すると、DNS 名が次のように表示されます。

![ワークスペースの作成][19]

次の図のように、「この Web サイトのセキュリティ証明書には問題があります」(Internet Explorer) または「この接続ではプライバシーが保護されません」 (Chrome) という警告が表示されます。 Internet Explorer の場合は **[このサイトの閲覧を続行する (推奨されません)]**、Chrome の場合は **[詳細設定]** の **[&#60;DNS 名*> にアクセスする (安全ではありません)]** をクリックして続行します。 次に、既に指定したパスワードを入力して IPython Notebook にアクセスします。

Internet Explorer:
![Create workspace][20]

Chrome:
![Create workspace][21]

IPython Notebook にログオンした後、*DataScienceSamples* ディレクトリがブラウザーに表示されます。 このディレクトリには、ユーザーがデータ サイエンス タスクを実行するうえで役立つ、Microsoft が共有するサンプル IPython Notebook が含まれています。 これらのサンプル IPython Notebook がからチェック アウト [* * Github リポジトリ * *](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) IPython Notebook サーバーのセットアップ プロセス中に仮想マシンにします。 Microsoft はこのリポジトリを保守し、頻繁に更新しています。 ユーザーは Github リポジトリを参照して、最近更新されたサンプル IPython Notebook を入手することができます。
![Create workspace][18]

## <a name="upload"></a>手順 5: アップロード、既存の IPython Notebook をローカル マシンから IPython Notebook サーバー

IPython Notebook を使用すると、ユーザーはローカル マシン上の既存の IPython Notebook を仮想マシン上の IPython Notebook サーバーに簡単にアップロードできます。 ユーザーは Web ブラウザーで IPython Notebook にログオンした後、IPython Notebook のアップロード先となる**ディレクトリ**にクリックして移動します。 次に、**[ファイル エクスプローラー]** で、ローカル マシンからアップロードする IPython Notebook の .ipynb ファイルを選択し、Web ブラウザーの IPython Notebook ディレクトリにドラッグ アンド ドロップします。 **[アップロード]** ボタンをクリックすると、.ipynb ファイルが IPython Notebook サーバーにアップロードされます。 その後、他のユーザーは自分の Web ブラウザーからそれを使用できるようになります。

![Create workspace][22]

![Create workspace][23]


## <a name="shutdown"></a>シャット ダウンしていないときに仮想マシンの割り当てを解除

Azure Virtual Machines の料金は**従量課金制**です。 仮想マシンを使用しないときに課金されないようにするには、未使用時に **[停止 (割り当て解除)]** 状態にする必要があります。
> [AZURE.NOTE] (Windows 電源オプションを使用して) 仮想マシン内部から VM をシャット ダウンすると、その VM は停止しますが、割り当てられた状態のままになります。 課金を続行しないことを確認するには、常に仮想マシンを停止、 [Azure Classic Portal](http://manage.windowsazure.com/)します。 また、Powershell で VM を停止することもできます。その場合、"PostShutdownAction" を "StoppedDeallocated" に設定して **ShutdownRoleOperation** を呼び出します。

仮想マシンをシャット ダウンして割り当て解除するには、次のようにします。

1. ログイン、 [Azure Classic Portal](http://manage.windowsazure.com/) 、アカウントを使用します。

2. 左側のナビゲーション バーから **[仮想マシン]** を選択します。

3. 仮想マシンの一覧で、仮想マシンの名前をクリックして **[ダッシュボード]** ページに移動します。

4. ページの下部にある **[シャット ダウン]** をクリックします。

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

