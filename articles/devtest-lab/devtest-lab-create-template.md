    <properties
    pageTitle="Create VM templates | Microsoft Azure"
    description="Learn how to create VM templates from VHD images"
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

# VM テンプレートの作成

## 概要

取得したら [ラボを作成した](devtest-lab-create-lab.md), 、ことができます [をラボに Vm を追加](devtest-lab-add-vm-with-artifacts.md) VM テンプレートの一覧からです。 この記事では、バーチャル ハード ディスク (VHD) イメージ ファイルをアップロードし、VM の作成に使用するテンプレートとして構成する方法について説明します。 VHD イメージに慣れていない場合を参照してください、 [の作成とアップロード Azure への Windows Server VHD](../virtual-machines-create-upload-vhd-windows-server.md) VHD イメージを作成する方法についての記事です。 VHD イメージを作成するか、VHD イメージへのアクセス権を取得したら、この記事の手順に従って VHD イメージをアップロードし、イメージからテンプレートを作成します。

## VM テンプレートの作成

1. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com)します。

1. タップ **参照**, 、順にタップ **DevTest ラボ** リストからです。

1. ラボの一覧で目的のラボをタップします。  

1. [ラボ] ブレードでタップ **設定**します。

    ![ラボの設定](./media/devtest-lab-create-template/lab-blade-settings.png)

1. 演習 **設定** ブレードで、tap **テンプレート**します。

    ![[テンプレート] オプション](./media/devtest-lab-create-template/lab-blade-settings-templates.png)

1.  **テンプレート** ブレードで、tap **+ テンプレート**します。

    ![テンプレートの追加](./media/devtest-lab-create-template/add-template.png)

1.  **Add Template** ブレード。

    1. テンプレートの名前を入力します。 この名前は、新しい VM を作成するときにテンプレートの一覧に表示されます。

    1. テンプレートの説明を入力します。 この説明は、新しい VM を作成するときにテンプレートの一覧に表示されます。

    1. タップ **イメージ**します。

    1. 必要なイメージが表示されていない追加する場合は、 [新しいテンプレート イメージの追加](#add-a-new-template-image) セクション、および完了すると、ここに戻っています。

    1. 目的のイメージを選択します。

    1. タップ **OK** を閉じる、 **テンプレートの追加** ブレードです。

1. タップ **OS 構成**します。

1.  **OS 構成** ] タブで、いずれかを選択 **Windows** または **Linux**です。

1. 場合 **Windows** は選択すると、チェック ボックスを使用して指定するかどうか *Sysprep* 、マシンで実行されています。

1. 入力、 **ユーザー名** マシン用です。

1. 入力、 **パスワード** マシン用です。 **注:** パスワードがクリア テキストで表示されます。

1. タップ **[ok]** を閉じる、 **OS 構成** ブレードです。

1. 指定の **場所**します。

1. タップ **OK** テンプレートを作成します。

##新しいテンプレート イメージの追加

新しいテンプレート イメージを追加するには、VHD イメージ ファイルにアクセスできる必要があります。

1.  **テンプレート イメージの追加** ブレードで、tap **PowerShell を使用してイメージをアップロード**します。

    ![イメージのアップロード](./media/devtest-lab-create-template/upload-image-using-psh.png)

1. 次のブレードには、VHD イメージ ファイルを Azure サブスクリプションにアップロードする PowerShell スクリプトの変更と実行の手順が表示されます。 **注:** このプロセスを接続の速度とイメージ ファイルのサイズによっては時間がかかることができます。

##次のステップ

次の手順で、バーチャル マシンを作成するときに使用する VM テンプレートを追加した後 [DevTest ラボに VM を追加](devtest-lab-add-vm-with-artifacts)します。

