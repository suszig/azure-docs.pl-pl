<properties 
    pageTitle="Linux 仮想マシンでの root 権限の使用 | Microsoft Azure" 
    description="Azure 上の Linux 仮想マシンで root 権限を使用する方法について説明します。" 
    services="virtual-machines" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/29/2015" 
    ms.author="szark"/>



# Azure 上の Linux 仮想マシンでの root 権限の使用

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

既定では、 `ルート` ユーザーは Azure で Linux 仮想マシンで無効にします。 ユーザーを使用して昇格した特権でコマンドを実行することができます、 `sudo` コマンドです。 ただし、システムがプロビジョニングされている方法によって操作が異なります。

1. **SSH キーとパスワードまたはパスワードのみ** -仮想マシンが証明書でプロビジョニングされました (`します。CER` ファイル) または SSH キーだけでなく、パスワード、またはユーザー名とパスワードのみです。 ここで `sudo` コマンドを実行する前に、ユーザーのパスワードが要求されます。

2. **SSH キーのみ** -証明書で、仮想マシンがプロビジョニングされている (`.cer`, 、`.pem`, 、または `.pub` ファイル) または SSH キー、パスワードがないです。 ここで `sudo` **されません** コマンドを実行する前に、ユーザーのパスワードの入力を求める。


## SSH キーとパスワード、またはパスワードのみ

SSH キーまたはパスワード認証を使用して Linux 仮想マシンにログオンし、コマンドを使用して実行 `sudo`, など。

    # sudo <command>
    [sudo] password for azureuser:

この場合はパスワードの入力が求められます。 パスワードの入力後 `sudo` を指定してコマンドを実行 `ルート` 権限です。

編集することによって、パスワードなし sudo を有効することも、 `/etc/sudoers.d/waagent` ファイルなど。

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

この変更によって、"azureuser" ユーザーはパスワードなしで sudo を実行できます。

## SSH キーのみ

SSH キー認証を使用して Linux 仮想マシンにログオンし、コマンドを使用して実行 `sudo`, など。

    # sudo <command>

この場合はパスワードの入力が**求められません**。キーを押した後 `< 入力 >`, 、`sudo` を指定してコマンドを実行 `ルート` 権限です。







