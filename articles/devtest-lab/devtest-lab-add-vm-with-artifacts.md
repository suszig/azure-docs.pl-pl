    <properties
    pageTitle="Add a VM with artifacts to a DevTest Lab | Microsoft Azure"
    description="Create a new virtual machine with Artifacts in DevTest Lab."
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/01/2015"
    ms.author="tarcher"/>

# アーティファクトを含む VM の Azure DevTest ラボへの追加

## 概要

Azure の基本イメージ、またはラボにアップロードしたイメージから、DevTest ラボに仮想マシンを作成します。

DevTest ラボ *アーティファクト* VM が作成されるときに実行されるアクションを指定できます。 アーティファクトのアクションでは、Powershell や Bash コマンドの実行、ソフトウェアのインストールなどの手順を実行することができます。 成果物 *パラメーター* 特定のシナリオでは、成果物をカスタマイズできます。

ラボには、パブリック DevTest ラボ アーティファクト リポジトリのアーティファクトと、独自のアーティファクト リポジトリに作成または追加されたアーティファクトが含まれます。

この記事では、アーティファクトを使用する VM をラボに作成する方法を説明します。

## アーティファクトを含む VM の追加

1. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com)します。

1. タップ **参照**, 、順にタップ **DevTest ラボ** リストからです。

1. ラボの一覧で、新しい VM を作成するラボをタップします。  

1. ラボのブレードで、[タップ **+ ラボ VM** 次の図に示すようにします。  
    ![DevTest ラボのホーム ブレード](./media/devtest-lab-add-vm-with-artifacts/devtestlab-home-blade-add-vm.png)

1.  **ラボ VM** ブレードで新しいバーチャル マシンの名前を入力、 **ラボ VM 名** テキスト ボックスです。

1. タップ **基本]、[必要な設定を構成する** し、VM の基本イメージを選択します。

    ![Lab VM settings](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-lab-vm-blade-1.png)  

1. 基本イメージを選択した後、 **ラボ VM** を含める] ブレードが展開され、 **ユーザー名** と **パスワード** 項目。 入力、 **ユーザー名** を仮想マシン上で管理者特権が付与されます。  

    ![Expanded Lab VM blade](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-lab-vm-blade-2.png)

1. 入力、 **パスワード**します。

1. タップ **VM サイズ** プロセッサ コア、RAM のサイズ、および作成する仮想マシンのハード ドライブのサイズを指定する定義済みの項目のいずれかを選択します。

1. タップ **アーティファクト** を成果物のリストから選択して、基本イメージに追加するアイテムを構成します。 **注:** DevTest ラボの経験がないかに進んで、成果物を構成したかどうか、 [を選択すると、アイテムの構成](#configuring-an-artifact) セクションを完了するとここで返します。

1. タップ **作成** 、ラボを指定された VM を追加します。

1. ラボのブレードには、VM の作成のステータスが表示されます。として最初 **作成**, 、として、 **を実行している** VM が開始された後です。 VM に接続するに VM をタップし、タップして、VM のブレードから **接続**します。  

## アーティファクトの選択と構成

VM を作成するときに、タップして成果物を追加できます **アーティファクト** から、 **ラボ VM** ブレードです。 これが表示されます、 **成果物の追加** ブレードを追加し、公式の DevTest ラボ リポジトリから VM の成果物を構成することができます (**正式なリポジトリ**) およびチーム リポジトリから成果物です。

![Add Artifacts blade](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifact-blade.png)

**VM へのアーティファクトの追加**

VM に追加する各アーティファクトについて、次の手順を実行します。

1. 必要な成果物をタップして、 **成果物の追加** ブレードを成果物のパラメーターを指定することができますブレードを表示します。  

2. 必須のパラメーター値を入力します。必要に応じて、省略可能なパラメーターも入力します。  

3. タップ **追加** アイテムを追加してに戻る、 **成果物の追加** ブレードです。

**アーティファクトの実行順序の変更**

上部にある成果物の現在の数を示すリンクが表示されますを追加して、VM に成果物を構成すると、 **成果物の追加** ブレードです。 既定では、アーティファクトのアクションは VM に追加された順序で実行されます。 成果物が実行される順序を変更するだけでドラッグ アーティファクトの一覧で、アンド ドロップをタップして **OK** 完了します。  

**選択したアーティファクトの表示または変更**

選択したアーティファクトのパラメーターを表示または変更するには、次の手順を実行します。

1. 上部にある、 **成果物の追加** ブレードで、VM に追加されたアイテムの数を示すリンクをタップします。

    ![](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)

1. 表示または特定のアイテムのパラメーターを編集、そのアーティファクトをタップして、 **成果物を選択した** ブレードです。  

1. 必要な変更を行い、タップ **[ok]** を閉じる、 **成果物の追加** ブレードです。

1. タップ **[ok]** を閉じる、 **成果物を選択した** ブレードです。

1. タップ **OK** を閉じる、 **成果物の追加** ブレードです。      

## 次のステップ

学習方法 [VM 用のカスタムの成果物を作成](devtest-lab-artifact-author.md)します。

