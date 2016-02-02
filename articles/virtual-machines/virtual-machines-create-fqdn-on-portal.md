<properties
   pageTitle="Azure ポータルでの VM の FQDN の作成 | Microsoft Azure"
   description="Azure ポータルで仮想マシンに基づいて、リソース マネージャーの完全修飾ドメイン名 (FQDN) を作成する方法を説明します。"
   services="virtual-machines"
   documentationCenter=""
   authors="dsk-2015"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/21/2015"
   ms.author="dkshir"/>


# Azure ポータルでの完全修飾ドメイン名の作成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。


内の仮想マシンを作成する場合、 [Azure ポータル](https://portal.azure.com) を使用して、 **リソース マネージャー** 配置モデル、ポータルが、仮想マシンのパブリック IP リソースを作成します。 この IP アドレスを使用して、仮想マシンにリモートでアクセスできます。 ただし、ポータルは作成されません、 [完全修飾ドメイン名](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) または既定では、FQDN。 IP アドレスよりも FQDN の方が記憶および使用が簡単なため、この記事では仮想マシンに FQDN を追加する方法について説明します。

この記事では、ポータルでサブスクリプションにログインし、**リソース マネージャー** を使用して、使用可能なイメージで仮想マシンを作成していることを前提としています。 仮想マシンの起動後、次の手順に従います。

1.  ポータルで仮想マシンの設定を表示し、パブリック IP アドレスをクリックします。

    ![IP リソースの検索](media/virtual-machines-create-fqdn-on-portal/locatePublicIP.PNG)

2.  仮想マシンとパブリック IP の**関連付けを解除**します。 この時点では、ドメイン名はまだ表示されません。 **[はい]** ボタンをクリックすると、数秒後に関連付け解除が完了します。

    ![IP リソースの関連付け解除](media/virtual-machines-create-fqdn-on-portal/dissociateIP.PNG)

    次の手順に従って、このパブリック IP を仮想マシンに関連付けます。 パブリック IP が _Dynamic パブリック IP_ の場合は、IPV4 アドレスと FQDN の構成後に 1 つ割り当てられる新しいを失われます。

3.  **[関連付けの解除]** ボタンがグレー表示になったら、パブリック IP の**[すべての設定]** セクションをクリックし、**[構成]** タブを開きます。 目的の DNS 名のラベルを入力します。 この構成を**保存**します。

    ![DNS 名のラベルの入力](media/virtual-machines-create-fqdn-on-portal/dnsNameLabel.PNG)

4.  ポータルの仮想マシン ブレードに戻り、仮想マシンの **[すべての設定]** をクリックします。 **[ネットワーク インターフェイス]** タブを開き、仮想マシンに関連付けられたネットワーク インターフェイス リソースをクリックします。 ポータルで **[ネットワーク インターフェイス]** ブレードが開きます。

    ![ネットワーク インターフェイスを開く](media/virtual-machines-create-fqdn-on-portal/openNetworkInterface.PNG)

5.  ネットワーク インターフェイスの **[パブリック IP アドレス]** フィールドが空白であることを確認します。 このネットワーク インターフェイスの **[すべての設定]** セクションをクリックし、**[IP アドレス]** タブを開きます。 **[IP アドレス]** ブレードで **[パブリック IP アドレス]** フィールドの **[有効]** をクリックします。 **[IP アドレス構成の必須設定]** タブを選択し、前の手順で関連付け解除した既定の IP を選択します。 **[保存]** をクリックします。 元の IP リソースが追加されるまで、少し時間がかかる場合があります。

    ![IP リソースの構成](media/virtual-machines-create-fqdn-on-portal/configureIP.PNG)

6.  他のすべてのブレードを閉じ、**[仮想マシン]** ブレードに戻ります。 設定でパブリック IP リソースをクリックします。 パブリック IP ブレードの **[DNS 名]** に目的の FQDN が表示されていることを確認してください。

    ![FQDN が作成される](media/virtual-machines-create-fqdn-on-portal/fqdnCreated.PNG)

    この DNS 名を使用して、仮想マシンにリモートで接続できるようになります。 などを使用して `SSH adminuser@testdnslabel.eastus.cloudapp.azure.com`, を完全修飾ドメインを持つ Linux 仮想マシンに接続するときに、名前 `testdnslabel.eastus.cloudapp.azure.com` のユーザー名と `adminuser`します。





