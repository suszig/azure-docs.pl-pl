<properties
pageTitle="Oracle VM イメージを使用するうえでの考慮事項 | Microsoft Azure"
description="デプロイする前に、Azure の Windows Server 上の Oracle VM でサポートされた考慮事項と制限事項を確認してください。"
services="virtual-machines"
documentationCenter=""
manager=""
authors="bbenz"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="06/22/2015"
ms.author="bbenz" />

#Oracle 仮想マシンのイメージに関するその他の考慮事項


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


この記事では、オペレーティング システムとして Windows Server を利用し、Microsoft 提供の Oracle ソフトウェア イメージに基づく Oracle 仮想マシンを Azure で実行することに関する考慮事項について説明します。  

-  Oracle データベース仮想マシン イメージ
-  Oracle WebLogic Server 仮想マシン イメージ
-  Oracle JDK 仮想マシンのイメージ

##Oracle データベース仮想マシン イメージ
### クラスタリング (RAC) はサポートされていません

現在、Azure では Oracle Database の Oracle Real Application Clusters (RAC) はサポートされていません。 Oracle Database のスタンドアロン インスタンスのみ使用可能です。 これは、Azure では現在のところ、複数の仮想マシンインスタンス間での仮想ディスク共有 (読み取り/書き込み) がサポートされていないためです。 マルチキャスト UDP もサポートされていません。

### 静的内部 IP はありません

Azure は各仮想マシンに内部 IP アドレスを割り当てます。 仮想マシンが仮想ネットワークに含まれていない限り、その仮想マシンの IP アドレスは動的であり、仮想マシンを再起動した後に変更される可能性があります。 Oracle データベースは静的な IP アドレスを要求するため、問題が生じることがあります。 こういった問題を避けるために、仮想マシンを Azure Virtual Network へ追加することを検討してください。 参照してください [仮想ネットワーク](http://azure.microsoft.com/documentation/services/virtual-network/) と [Azure で仮想ネットワークの作成](create-virtual-network.md) の詳細。

### 接続ディスクの設定オプション

仮想マシンのオペレーティングシステムのディスク上、またはデータ ディスクとも呼ばれる接続ディスクのいずれかにデータファイルを配置することができます。 接続ディスクはオペレーティング システムのディスクよりもパフォーマンスに優れ、サイズを柔軟に構築できます。 オペレーティング システム ディスクは、10 ギガバイト (GB) 以下のデータベースにしか推奨できない場合があります。

接続ディスクは Azure Blob Storage サービスを利用します。 各ディスクは理論上、毎秒最大約 500 回 の入出力の操作 (IOPS) が可能です。 最初は接続ディスクのパフォーマンスが最適でない場合があります。IOPS パフォーマンスは約 60 ～ 90 分の連続操作 「バーンイン」期間の後に大幅に改善される可能性があります。 その後、ディスクのアイドル状態が続く場合、もう一度連続操作 (バーンイン期間) するまで IOPS パフォーマンスは低下する可能性があります。 つまり、ディスクがアクティブであるほど、最適な IOPS パフォーマンスに近づく可能性が高くなります。

最も簡単な方法は 1 つのディスクを仮想マシンに接続し、そのディスクにデータベース ファイルを置くことですが、その方法はパフォーマンスの観点では最も制約があります。 代わりに、多くの場合、複数の接続ディスクを使い、それらの間でデータベースのデータを分散させ、Oracle Automatic Storage Management (ASM) を利用すると、効果的に IOPS パフォーマンスを改善できます 参照してください [Oracle 自動記憶域の概要](http://www.oracle.com/technetwork/database/index-100339.html) の詳細。 オペレーティング システム レベルで複数のディスクのストライピングを使用することができますが、IOPS パフォーマンスの向上については不明なため、推奨されてはいません。

読み込み操作、またはデータベースへの書き込み操作のいずれを重視するかに基づいて、複数のディスクを接続する場合に向けて 2 つの手法を検討してください。

- **Oracle ASM 単独で** 書き込み操作のパフォーマンスを向上させるが IOPS が下がります Windows Server 2012 記憶域プールを使用したアプローチと比較して読み取り操作が発生する可能性があります。 次の図は理論的にこの配置を示したものです。  
    ![](media/virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images/image2.png)

- **Oracle ASM と Windows Server 2012 ストレージ プール** 読み取り操作 IOPS パフォーマンス、データベースは、主に読み取り操作を実行する場合、または書き込み操作より読み取り操作のパフォーマンスを重視する場合に発生する可能性があります。 Windows Server 2012 オペレーティング システムに基づくイメージが必要です。 参照してください [スタンドアロン サーバー上の記憶域スペースを展開する](http://technet.microsoft.com/library/jj822938.aspx) の詳細については、記憶域プール用です。 この配置の場合、接続ディスクの 2 つの等しいサブセットが最初に 2 つの記憶域プール ボリュームの物理ディスクとしてまとめて「ストライプ化」され、そのボリュームが ASM ディスク グループに追加されます。 次の図は理論的にこの配置を示したものです。  

    ![](media/virtual-machines-miscellaneous-considerations-oracle-virtual-machine-images/image3.png)  

>[AZURE.IMPORTANT] 書き込みのパフォーマンスと、個別に読み取りのパフォーマンスのトレードオフを評価します。 これらの方法を使用すると、実際の結果が変わる可能性があります。

### 高可用性と障害復旧に関する考慮

Azure 仮想マシンで Oracle データベースを使用する場合、いかなるダウンタイムも回避するために高可用性と障害復旧ソリューションを実装する責任があります。 また、ご自身のデータやアプリケーションをバックアップする責任も負うことになります。

使用して (RAC なし) の Oracle Database Enterprise Edition Azure での高可用性と災害復旧を実現できる [Data Guard, Active Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), 、または [Oracle Golden Gate](http://www.oracle.com/technetwork/middleware/goldengate), 、2 つのデータベースを 2 つの個別の仮想マシン。 両方の仮想マシンが同じでなければなりません [クラウド サービス](cloud-services-connect-virtual-machine.md) と同じ [仮想ネットワーク](http://azure.microsoft.com/documentation/services/virtual-network/) にプライベートの固定 IP アドレスを介して相互にアクセスできることを確認します。  また、同じ仮想マシンを配置する推奨 [可用性セット](manage-availability-virtual-machines.md) Azure が別個のフォールト ドメインに配置し、アップグレード ドメインを許可するようにします。 同じクラウド サービス上にある仮想マシンのみを、同じ可用性セットに含めることができます。 それぞれの仮想マシンには、少なくとも 2 GB のメモリと 5 GB のディスク領域が必要です。

Oracle Data Guard では、1 つの仮想マシンにプライマリ データベース、別の仮想マシンにセカンダリ データベース (待機)、そしてその間に一方向のレプリケーションセットを配置することで高可用性を実現できます。 データベースのコピーへのアクセスを結果として読み込みます。 Oracle GoldenGate では、2 つのデータベース間に双方向レプリケーションを構成することができます。 これらのツールを使用してデータベースの高可用性ソリューションを設定する方法については、次を参照してください。 [Active Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) と [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) Oracle の web サイトのドキュメントです。 使用することがでく必要がある読み取り/書き込みアクセス場合、データベースのコピーに、 [Oracle Active Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html)します。

##Oracle WebLogic Server 仮想マシン イメージ

-  **クラスタ リングは Enterprise Edition でのみサポートされます。**WebLogic Server (具体的には、Windows Server をオペレーティング システムとするイメージ) の Microsoft ライセンス イメージを使用している場合は、WebLogic Server の Enterprise エディションを使用している場合にのみ、WebLogic クラスタリングを使用するライセンスが付与されます。 WebLogic Server Standard Edition の場合、クラスタリングを使用しないでください。

-  **接続タイムアウト:** Azure が 4 分続くと、開いている接続を閉じることがある場合は、アプリケーションが別の Azure クラウド サービス (データベース層のサービスなど) のパブリック エンドポイントへの接続に依存します。 無活動時間がこの上限を超えると、接続が無効になるため、接続プールに依存している機能やアプリケーションに影響が及ぼされることがあります。 アプリケーションに影響がある場合は、接続プール上の「キープアライブ」 ロジックの有効化を検討してください。

    エンドポイントが場合は *内部* を Azure クラウド サービス デプロイ (内でスタンドアロンのデータベース仮想マシンなど、 *同じ* WebLogic 仮想マシンとクラウド サービス)、接続は直接的および Azure ロード バランサーに依存せず、したがって、接続タイムアウトの対象はありませんし、します。

-  **UDP マルチキャストはサポートされていません。**Azure は UDP ユニキャストをサポートしていますが、マルチキャストおよびブロードキャストはサポートしていません。 WebLogic Server は Azure の UDP ユニキャスト機能に依存できます。 UDP ユニキャストへの依存で最適な結果を得るための、WebLogic クラスターのサイズを静的に保ち、クラスターに 10 以上の管理サーバーを置かないことを推奨します。

-  **WebLogic Server は T3 アクセスに (Enterprise JavaBeans を使用する場合など) を同一にする、パブリックとプライベート ポートが必要です。**サービス層 (EJB) アプリケーションがという名前のクラウド サービス内の 2 つ以上の管理対象サーバーで構成される WebLogic Server クラスター上で実行されている多層シナリオを考えてみます **SLWLS**します。 クライアント層は別のクラウド サービスにあり、サービス層にある EJB を呼び出すためにシンプルな Java プログラムを実行します。 サービス層の負荷を分散する必要があるため、負荷分散されたパブリックなエンドポイントを WebLogic Server クラスター内の仮想マシンに作成する必要があります。 そのエンドポイント用に指定したプライベートポートがパブリックポート (7006:7008 など) と異なる場合、次のようなエラーが発生します。

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    どのようなリモート T3 アクセスにおいても、WebLogic Server に対してロード バランサー ポートおよび WebLogic 管理サーバーのポートが同じでなければいけないためです。 上記の場合、クライアントはポート 7006 (ロード バランサー ポート) へ接続し、管理サーバーは 7008 (プライベート ポート) で待機します。 この制限は、 HTTP  ではなく T3 アクセスにのみ適用されることに注意してください。

    この問題を避けるには、次の回避策のいずれかを実施してください。

    -  T3 アクセス専用の負荷分散エンドポイントに対して、同じプライベートおよびパブリック ポート番号を使用します。

    -  WebLogic Server を開始する場合、次の JVM パラメーターを含めてください。

            -Dweblogic.rjvm.enableprotocolswitch=true

関連情報については、サポート技術情報の記事を参照してください **860340.1** で <http://support.oracle.com>します。

-  **動的なクラスタ リングと負荷分散の制限事項です。**WebLogic Server で動的なクラスターを使用し、Azure 上の単一のパブリック負荷分散エンドポイントでそれを公開すると仮定します。 これは、それぞれの管理サーバーに固定のポート番号を使用し (範囲から動的に割り当てられていない)、管理者が追跡記録しているマシンより多く管理サーバーを起動しない (つまり、1 つの仮想マシンに対する管理サーバーが複数にならない) 限り、実現できます。 設定の結果、起動する WebLogic Server の数が仮想マシンより多くなる場合 (つまり、複数の WebLogic Server インスタンスが同じ仮想マシンを共有する場合)、指定したポート番号に複数の WebLogic Server インスタンス サーバーをバインドすることはできません。その仮想マシンのその他の WebLogic Server インスタンスは失敗します。

    一方、管理サーバーに個別のポート番号を自動的に割り当てるように管理サーバーを設定した場合、負荷分散はできません。そのような設定では、単一のパブリック ポートから複数のプライベート ポートにマッピングする必要がありますが、Azure ではサポートされていないためです。

-  **仮想マシン上の Weblogic サーバーの複数のインスタンス。**仮想マシンが十分に大きい場合、デプロイ要件によりますが、同じ仮想マシン上で複数の WebLogic Server のインスタンスを実行することも検討できます。 たとえば、2 つのコアを持つ中程度のサイズの仮想マシンでは、WebLogic Server で 2 つのインスタンスを実行できます。 ただし、1 つの仮想マシンで複数の WebLogic Server インスタンスを実行する場合のように、単一障害点をアーキテクチャに導入することは避けることが推奨されます。 2 つ以上の仮想マシンを使うほうがより優れた手法であり、各仮想マシンは複数の WebLogic Server のインスタンスを実行できるようになります。 それでもなお、これらの WebLogic Server のインスタンスは同じクラスターの一部にすることができます。 ただし、現在は、Azure のロード バランサーを利用する場合、負荷を分散するサーバーを個別の仮想マシン間で配布する必要があるため、同じ仮想マシン内の WebLogic Server のデプロイによって公開されるエンドポイントの負荷を Azure で分散することはできません。

##Oracle JDK 仮想マシンのイメージ

-  **JDK 6 および 7 の最新アップデート。**パブリックをサポートする Java の最新バージョン (現在は Java 8) の使用を推奨していますが、Azure には JDK 6 および JDK 7 のイメージもご利用いただけます。 これは JDK 8 へのアップグレードの準備が整っていない従来のアプリケーションのための対処としてです。 一般向けには以前の JDK イメージの一般公開は終了していますが、Microsoft と Oracle の業務提携により、Azure 提供の JDK 6 および 7 のイメージには、通常、Oracle のサポート対象顧客の選ばれたグループにのみ Oracle が提供する最近の非公開更新が含まれます。 新しいバージョンの JDK イメージは JDK 6 および 7 の更新版がリリースされた後に利用可能になります。

    この JDK 6 および JDK 7 のイメージで利用可能な JDK 、そしてそこから派生した仮想マシンおよびイメージは Azure 内でのみ使用できます。

-  **64 ビット JDK。**Azure によって提供される Oracle WebLogic Server 仮想マシンおよび Oracle JDK 仮想マシンのイメージには、Windows Server と JDK 両方の 64 ビット版が含まれています。

##その他のリソース
[Azure の oracle 仮想マシン イメージ](virtual-machines-oracle-list-oracle-virtual-machine-images.md)

