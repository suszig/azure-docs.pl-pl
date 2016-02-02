<properties
    pageTitle="GitHub から Azure Linux エージェントを更新する | Microsoft Azure"
    description="Azure の Linux VM で使用する Azure Linux エージェントを Github の最新バージョンに更新する方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2015"
    ms.author="mingzhan"/>



# VM 上の Azure Linux エージェントを GitHub で最新バージョンに更新する方法

更新する、 [Azure Linux エージェント](https://github.com/Azure/WALinuxAgent) Azure での Linux VM で既にでなければなりません。

1. Linux VM が Azure で実行されている
2. SSH を使用してその Linux VM に接続している

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Windows コンピューターでこのタスクを実行する場合は、Putty を使用して Linux マシンに SSH 接続できます。 詳細については、次を参照してください。 [を実行している Linux の仮想マシンにログオンする方法](virtual-machines-linux-how-to-log-on.md)します。

Azure での動作保証済み Linux ディストリビューションは、そのリポジトリに Azure Linux エージェント パッケージを格納しているため、可能な場合は、そのディストリビューションのリポジトリを確認して最新のバージョンをインストールしてください。

Ubuntu の場合は、次のように入力するだけでかまいません。

    #sudo apt-get install walinuxagent

また CentOS に次のように入力します。

    #sudo yum install waagent

Oracle linux の場合は、必ずファイルで有効にするアドオンのリポジトリ `/etc/yum.repo.d/public-yum-ol6.repo` または `/etc/yum.repo.d/public-yum-ol7.repo`, を入力します。

    #sudo yum install WALinuxAgent

通常これは、必要がありますが、何らかの理由は、https://github.com から直接インストールする必要がある場合は、次の手順を使用してすべてです。


## wget のインストール

SSH を使用して VM にログインします。

」と入力して、wget (Redhat、CentOS、Oracle Linux version 6.4、6.5 のように既定ではインストールされない一部のディストリビューションがあります) をインストール `#sudo yum wget をインストールする` コマンド行にします。


## 最新版のダウンロード

開いている [GitHub の Azure Linux エージェントのリリース](https://github.com/Azure/WALinuxAgent/releases) web ページおよび最新のバージョン番号を確認します。 (」と入力して、現在のバージョンを検索できます `#waagent--バージョン`.)

### バージョン 2.0.x の場合は次のように入力します:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   次は、バージョン 2.0.14 を例として使用しています。

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### バージョン 2.1.x 以降の場合は次のように入力します:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   次の行は、バージョン 2.1.0 を例として使用しています。

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## Linux エージェントのインストール

### バージョン 2.0.x の場合は次を使用します:

 waagent を実行可能にする

    #chmod +x waagent

 新しい実行可能ファイルを /usr/sbin にコピーする

  ほとんどの Linux では、次のコマンドを使用します。

    #sudo cp waagent /usr/sbin

  CoreOs の場合は、次のコマンドを使用します。

    #sudo cp waagent /usr/share/oem/bin/

  Azure Linux エージェントを新規にインストールの場合は、次を実行します。

    #sudo /usr/sbin/waagent -install -verbose

### バージョン 2.1.x の場合は次を使用します:

パッケージをインストールする必要がある場合があります `setuptools` 最初を参照してください [ここ](https://pypi.python.org/pypi/setuptools)します。 次に以下を実行します。

    #sudo python setup.py install

## waagent サービスを再起動する

ほとんどの Linux ディストリビューションでは、次のコマンドを使用します。

    #sudo service waagent restart

Ubuntu の場合は、次のコマンドを使用します。

    #sudo service walinuxagent restart

CoreOs の場合は、次のコマンドを使用します。

    #sudo systemctl restart waagent

## Azure Linux エージェントのバージョンを確認する

    #waagent -version

CoreOS では、上記のコマンドが機能しない場合があります。

これで、Linux エージェントのバージョンが新しいバージョンに更新されたことが確認できます。

Azure Linux エージェントの詳細については、次を参照してください。 [Azure Linux エージェントの README](https://github.com/Azure/WALinuxAgent)します。





