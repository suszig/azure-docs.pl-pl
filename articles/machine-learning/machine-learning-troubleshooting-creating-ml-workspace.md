<properties
    pageTitle="トラブルシューティング: Machine Learning ワークスペースの作成と接続 | Microsoft Azure"
    description="Azure Machine Learning ワークスペースの作成と接続に関する一般的な問題の解決策"
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


# トラブルシューティング ガイド: Machine Learning ワークスペースの作成と接続

このガイドでは、Azure Machine Learning ワークスペースの設定に伴い、よく生じることのある問題の解決策を説明します。

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## ワークスペースの所有者

新しい Machine Learning ワークスペースを作成するときに、ワークスペースの所有者] フィールドに入力する ID は有効な Microsoft アカウント (旧 Windows Live ID)、たとえば、john-contoso@live.com や john-contoso@hotmail.com をする必要があります。 会社の電子メール アカウントなどの Microsoft アカウント以外のアカウントを指定することはできません。 無料の Microsoft アカウントを作成するには [www.live.com](http://www.live.com)します。

ワークスペースを作成する Azure 旧ポータルにサインインするために使用するアカウントに自動的にいないことをするアクセス許可に注意してください *開く* そのワークスペースの所有者としてそのアカウントを指定しない限りです。 Machine Learning Studio でワークスペースを開くには、ワークスペースの所有者として定義された Microsoft アカウントにサインインするか、所有者からワークスペースへの参加の招待を受け取る必要があります。 ことはできますが、Azure Classic Portal から *管理* 、ワークスペースでは、所有者を変更して、アクセスを構成する機能が含まれています。

ワークスペースの管理方法の詳細については、次を参照してください。 [Manage an Azure Machine Learning workspace]します。

[Manage an Azure Machine Learning workspace]: machine-learning-manage-workspace.md

## 許可されたリージョン

現在、Machine Learning は一部のリージョンでのみ利用できます。 サブスクリプションに対象のリージョンが含まれていない場合は、"許可されたリージョンにサブスクリプションがありません" というエラー メッセージが表示されることがあります。

領域が、サブスクリプションに追加することを要求するには、次のように選択します。 **Microsoft サポートに問い合わせる** Azure クラシック ポータルでは、次のように選択します。 **課金** 問題の種類として、要求を送信する指示に従います。

![Microsoft サポートに問い合わせる][screen1]

## ストレージ アカウント

Machine Learning サービスでは、データを格納するためのストレージ アカウントが必要です。 既存のストレージ アカウントを使用するか、新しい Machine Learning ワークスペースの作成時に新しいストレージ アカウントを作成することができます (新しいストレージ アカウントを作成するためのクォータがある場合)。

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Create workspace][screen2]

新しい Machine Learning ワークスペースを作成した後、ワークスペースの所有者として指定した Microsoft アカウントを使用して Machine Learning Studio にサインインできます。 エラー メッセージ「ワークスペースが見つかりません」(次のスクリーン ショットに類似) が発生する場合は、次の手順に従ってブラウザーの Cookie を削除します。

![Workspace not found][screen3]

**ブラウザーの Cookie を削除するには**

Internet Explorer を使用する場合は、クリックして、 **ツール** 右上隅にあるボタンをクリックして **インターネット オプション**します。  

![インターネット オプション][screen4]

下にある、 **全般** ] タブ、[ **を削除しています.**

![[全般] タブ][screen5]

 **閲覧の履歴の削除** ] ダイアログ ボックスで確認 **Cookie や web サイトのデータ** が選択されているし、クリックして **削除**します。

![Cookie の削除][screen6]

ブラウザーを再起動しに移動し、cookie を削除した後、 [Microsoft Azure Machine Learning](https://studio.azureml.net) ページです。 ユーザー名とパスワードの入力が求められたら、ワークスペースの所有者として指定したアカウントと同じ Microsoft アカウントを入力します。

目的は、可能な限り円滑でシームレスに Machine Learning を体験していただくことです。 任意のコメントや問題を投稿してください、 [Azure Machine Learning フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) より良いサービスを提供するためです。

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png

