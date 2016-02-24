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

コラムでは、ポータルで、サブスクリプションにログインしてし、を使用して、使用可能なイメージを使用する仮想マシンを作成した、 **リソース マネージャー**します。 仮想マシンの起動後、次の手順に従います。

1.  ポータルで仮想マシンの設定を表示し、パブリック IP アドレスをクリックします。

    ![IP リソースの検索](media/virtual-machines-create-fqdn-on-portal/locatePublicIP.PNG)

2.  **関連付けを解除** バーチャル マシンからパブリック ip アドレスです。 この時点では、ドメイン名はまだ表示されません。 をクリックした後、 **はい** ] ボタン、dissociation を完了する前に、数秒かかります。

    ![IP リソースの関連付け解除](media/virtual-machines-create-fqdn-on-portal/dissociateIP.PNG)

    次の手順に従って、このパブリック IP を仮想マシンに関連付けます。 パブリック IP がある場合、 _動的パブリック IP_, 、IPV4 アドレスが失われ、FQDN を構成した後、新しいものが割り当てられます。

3.  1 回、 **Dissociate** グレーで表示] をクリックし、ボタンをクリックして、 **設定をすべて** のパブリック IP と開いているセクション、 **構成** ] タブをクリックします。 目的の DNS 名のラベルを入力します。 **保存** この構成します。

    ![DNS 名のラベルの入力](media/virtual-machines-create-fqdn-on-portal/dnsNameLabel.PNG)

4.  ポータルで仮想マシンのブレードに戻り、 **設定をすべて** 、仮想マシンのです。 開いている、 **ネットワーク インターフェイス** タブし、このバーチャル マシンに関連付けられているネットワーク インターフェイス リソースをクリックします。 開き、 **ネットワーク インターフェイス** ポータルのブレードでします。

    ![ネットワーク インターフェイスを開く](media/virtual-machines-create-fqdn-on-portal/openNetworkInterface.PNG)

5.  なお、 **パブリック IP アドレス** フィールドは、ネットワーク インターフェイスが空白です。 クリックして、 **すべての設定** このネットワーク インターフェイスと開いているセクション、 **IP アドレス** ] タブをクリックします。  **IP アドレス** ブレードで、をクリックして **有効** の **パブリック IP アドレス** フィールドです。 選択、 **IP アドレス構成の設定に必要な** タブし、するが以前関連付けを解除した既定の IP を選択します。 クリックして **保存**します。 元の IP リソースが追加されるまで、少し時間がかかる場合があります。

    ![IP リソースの構成](media/virtual-machines-create-fqdn-on-portal/configureIP.PNG)

6.  その他のすべてのブレードを閉じてに戻り、 **仮想マシン** ブレードです。 設定でパブリック IP リソースをクリックします。 パブリック IP のブレードに必要な FQDN として表示されることを確認、 **DNS 名**します。

    ![FQDN が作成される](media/virtual-machines-create-fqdn-on-portal/fqdnCreated.PNG)

    この DNS 名を使用して、仮想マシンにリモートで接続できるようになります。 たとえば、`SSH adminuser@testdnslabel.eastus.cloudapp.azure.com` を使用して、完全修飾ドメイン名が `testdnslabel.eastus.cloudapp.azure.com` でユーザー名が `adminuser` の Linux 仮想マシンに接続できます。

