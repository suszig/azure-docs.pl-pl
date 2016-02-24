<properties
    pageTitle="動作保証済み Linux ディストリビューション | Microsoft Azure"
    description="Azure での動作保証済み Linux ディストリビューションについて、Ubuntu、OpenLogic、Oracle、および SUSE に関する指針も含めて、説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management,azure-resource-manager"
    />

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="szark"/>



#Azure での動作保証済み Linux ディストリビューション

Azure ギャラリーまたは Marketplace にある Linux イメージは多くのパートナーから提供されており、Microsoft はさまざまな Linux コミュニティと協力して、動作保証済みディストリビューションの一覧をよりいっそう充実させようと努めています。 その間は、ギャラリーから使用できないディストリビューションの常に Bring Your-独自の Linux でできます指針に従う [このページ](virtual-machines-linux-create-upload-vhd.md)します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## サポートされているディストリビューションとバージョン ##

次の表に、Azure でサポートされている Linux ディストリビューションとバージョンを示します。

Hyper-V および Azure 用の Linux Integration Services (LIS) ドライバーは、Microsoft からアップストリームの Linux カーネルに直接提供されるカーネル モジュールです。  LIS ドライバーが、既定では、ディストリビューションのカーネルに組み込まれたか、または古い RHEL/CentOS ベースのディストリビューションは個別のダウンロードとして利用可能な [ここ](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)します。  参照してください [今回](virtual-machines-linux-create-upload-vhd-generic.md#linux-kernel-requirements) LIS ドライバーの詳細についてです。

Azure Linux エージェントは、Azure ギャラリーのイメージにあらかじめインストールされており、通常はディストリビューションのパッケージのリポジトリで入手できます。  ソース コードを参照して [GitHub](https://github.com/azure/walinuxagent)します。

ディストリビューション|バージョン|ドライバー|エージェント
---|---|---|---
CentOS by OpenLogic |CentOS 6.3+、7.0+| CentOS 6.3: [LIS ダウンロード](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 以上: カーネル内|: パッケージ <a href="http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/">OpenLogic リポジトリ"walinuxagent"<p><p>ソース コード: [GitHub](https://github.com/Azure/WALinuxAgent)
[CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/)|494.4.0+ |カーネル内|ソース コード: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent)
Credativ の Debian |Debian 7.9+、8.2+|カーネル内|パッケージ:"waagent"の下のリポジトリ内 <p><p>ソース コード: [GitHub](https://github.com/Azure/WALinuxAgent)
Oracle Linux| 6.4+、7.0+|カーネル内|パッケージ:"walinuxagent"リポジトリ内<p><p>ソース コード: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
Red Hat Enterprise Linux |RHEL 6.7+、7.1+|カーネル内|パッケージ:"walinuxagent"リポジトリ内 <p><p>ソース コード: [GitHub](https://github.com/Azure/WALinuxAgent)
SUSE Linux Enterprise |SLES 11 SP3+、SLES 12+、および  <p><p> SLES for SAP 11.3+ |カーネル内|: パッケージ [Cloud:tools](https://build.opensuse.org/project/show/Cloud:Tools) リポジトリ"walinuxagent"<p><p>ソース コード: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998)
openSUSE |openSUSE 13.1+|カーネル内|: パッケージ [Cloud:tools](https://build.opensuse.org/project/show/Cloud:Tools) リポジトリ"walinuxagent" <p><p>ソース コード: [GitHub](https://github.com/Azure/WALinuxAgent)
Ubuntu|Ubuntu 12.04、14.04、15.04、および 15.10|カーネル内|パッケージ:"walinuxagent"リポジトリ内 <p><p>ソース コード: [GitHub](https://github.com/Azure/WALinuxAgent)
## パートナー

### OpenLogic
[http://www.openlogic.com/azure](http://www.openlogic.com/azure)

OpenLogic は、クラウドおよびデータ センター向けの企業オープン ソース ソリューションの一流プロバイダーです。 OpenLogic は、多種多様な業界の一流企業数百社に対して、オープン ソース ソフトウェアを安全に入手し、サポートし、コントロールできるよう支援しています。 OpenLogic は、600 種類のオープン ソース パッケージについて、OpenLogic Expert Community に支えられた商用品質のテクニカル サポートと保証を提供しています。たとえば、CentOS を企業レベルでサポートし、Azure に CentOS ベースのイメージを提供する開設時パートナー様でもあります。

### CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

CoreOS Web サイトから入手:

*CoreOS はセキュリティ、一貫性、および信頼性に設計されています。 yum や apt でパッケージをインストールする代わりに、Linux コンテナーを使用して、より抽象的なレベルでサービスを管理します。 1 つまたは複数の CoreOS マシン上で実行可能なコンテナー内では、1 つのサービスのコードとすべての依存関係がパッケージ化されます。*


### Credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ は、無償のソフトウェアを使用したプロフェッショナルなソリューションの開発および実装を専門とする、独立したコンサルティングおよびサービス企業です。 大手のオープン ソース スペシャリストとして、Credativ のサポートを利用している多くの IT 部門から国際的に認知されています。 Credativ は現在、マイクロソフトと協力して、Azure で実行するために特別に設計され、プラットフォーム経由で簡単に管理できる Debian 8 (Jessie) および 7 より前の Debian (Wheezy) に対応する Debian イメージを準備しています。 Credativ では、自社のオープン ソース サポート センターを通じて、Azure 用の Debian イメージの長期にわたる保守および更新もサポートします。

### Oracle
[http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle の戦略は、パブリック クラウドとプライベート クラウドに対して広範なソリューションを用意し、顧客が Oracle のクラウドや他社のクラウドにおいて Oracle のソフトウェアを柔軟にデプロイできるようにすることです。  マイクロソフトと Oracle との提携により、お客様はマイクロソフトのパブリック クラウドおよびプライベート クラウドにおいて Oracle の保証とサポートの下で安心して Oracle のソフトウェアをデプロイできます。  Oracle のパブリックおよびプライベート クラウド ソリューションに対する取り組みと投資が変わることはありません。

### Red Hat
[http://www.redhat.com/en/partners/strategic-alliance/microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

世界をリードするオープン ソース ソリューションのプロバイダーである Red Hat は、90% 超の Fortune 500 企業が、ビジネス上の課題を解決したり、IT およびビジネス戦略を調整したり、将来のテクノロジを準備するのを支援しています。 Red Hat は、オープン ビジネス モデルおよび手頃な価格の予測可能なサブスクリプション モデルを使用して、安全なソリューションを提供することによってこれを実現しています。

### SUSE
[http://www.suse.com/suse-linux-enterprise-server-on-azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server on Azure は、クラウド コンピューティングで優れた信頼性とセキュリティを実現する実績あるプラットフォームです。 SUSE の多機能な Linux プラットフォームは Azure クラウド サービスとシームレスに統合され、管理の容易なクラウド環境を実現します。 SUSE Linux Enterprise Server には、1,800 社を超える独立系ソフトウェア ベンダーによる 9,200 本を超える公認アプリケーションが存在し、SUSE は、データ センターで実行されているワークロードを Azure に安心して自信を持ってデプロイできることを保証しています。

### Canonical
[http://www.ubuntu.com/cloud/azure](http://www.ubuntu.com/cloud/azure)

Canonical のエンジニア リングおよびオープン コミュニティ体制は、クライアント、サーバーおよびクラウド消費者向け個人クラウド サービスなど、コンピューティングにおける Ubuntu の成功を支えています。 Canonical は、Ubuntu に関して、携帯電話、タブレット、テレビ、およびデスクトップで一貫したインターフェイスを備え、携帯電話からクラウドまで統一されたフリー プラットフォームにするというビジョンを持っているため、Ubuntu はパブリック クラウド プロバイダーから家電メーカーに至るまで多様な企業が最初に選択するディストリビューションとなり、個々の技術者のお気に入りとなっています。

世界中に開発者と開発センターが存在しているため、Canonical は独自の地位を確立しており、ハードウェア メーカーやコンテンツ プロバイダー、ソフトウェア開発者と提携して、PC からサーバーや携帯機器まで、さまざまな Ubuntu ソリューションを市場に送り出しています。


