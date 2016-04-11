<properties
    pageTitle="Site Recovery で仮想マシンおよび物理サーバーを保護するための容量計画 | Microsoft Azure"
    description="Azure Site Recovery は、オンプレミスに配置されている仮想マシンと物理サーバーの Azure またはセカンダリ オンプレミス サイトへのレプリケーション、フェールオーバー、および復旧を調整します。" 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="12/14/2015" 
    ms.author="raynew"/>

# Azure Site Recovery で仮想マシンおよび物理サーバーを保護するための容量計画

Capacity Planner ツールを使用すると、Azure Site Recovery で HYPER-V VM、VMware VM、Windows および Linux 物理サーバーを保護するために必要な容量を算出できます。


## 概要

このツールでは、ソース環境やワークロードを分析したり、必要な帯域幅、ソースの場所で必要なサーバー リソース、ターゲットの場所に必要な (仮想マシンやストレージなどの) リソースを算出できます。 

このツールは、いくつかのモードで実行できます。

- **クイック計画**: ネットワークやサーバーの予測を取得するには、このモードでは、ツールが Vm、ディスク、ストレージ、および変更率の平均の数に基づいて実行します。
- **計画の詳細**: このモードでは、ツールを実行し、VM レベルでは、各ワークロードの詳細を指定します。 VM の互換性が分析され、ネットワークおよびサーバーが予測されます。

## 開始する前に

ツールを実行する前に、以下を行ってください。

1. VM、VM あたりのディスク数、ディスクあたりのストレージなど、環境の情報を収集します。
2. レプリケートされたデータの 1 日の変更 (チャーン) 率を識別します。 これを行うには、次の手順を実行します。

    - HYPER-V 仮想マシンをレプリケートする場合、ダウンロード、 [HYPER-V キャパシティ プランニング ツール](https://www.microsoft.com/download/details.aspx?id=39057) 変更率を取得します。 [詳細については](site-recovery-capacity-planning-for-hyper-v-replication.md) このツールの詳細。 このツールは 1 週間に渡って実行して平均をキャプチャすることをお勧めします。
    - VMware バーチャル マシンをレプリケートする場合に使用して、 [vSphere のキャパシティ プランニングのアプライアンス](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) チャーン率を確認します。
    - 物理サーバーをレプリケートする場合、手動で評価を行う必要があります。

## クイック プランナーの実行
1.  ダウンロードして開きます、 [Azure サイト回復 Capacity Planner](http://aka.ms/asr-capacity-planner-excel) ツールです。 マクロを実行する必要があるので、求められたら編集の有効化とコンテンツの有効化を選択します。 
2.   **プランナーの種類を選択して** 選択 **クイック プランナー** リスト ボックスからです。

    ![使用の開始](./media/site-recovery-capacity-planner/getting-started.png)

3.   **Capacity Planner** ワークシートは、必要な情報を入力します。 以下のスクリーンショットの赤の丸がついているすべてのフィールドに、入力を行う必要があります。

    -  **、シナリオの選択** 選択 **を Azure に HYPER-V** または **Azure への VMware/物理**します。
    -  **平均日次データ変化率 (%)** を使用して収集した情報に、 [HYPER-V キャパシティ プランニング ツール](site-recovery-capacity-planning-for-hyper-v-replication.md) または [vSphere のキャパシティ プランニングのアプライアンス](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance)します。  
    - **圧縮** VMware 仮想マシンまたは物理サーバーを Azure にレプリケートするときに提供される圧縮にのみ適用されます。 30% 以上を見積もりますが、必要に応じて設定を変更することができます。 HYPER-V の VM を Azure の圧縮にレプリケートする場合、Riverbed などのサードパーティのアプライアンスを使用できます。 
    -   **保有入力** レプリカを保持する期間を指定します。 VMware または物理サーバーをレプリケートする場合は、日数で値を入力します。 Hyper-V をレプリケートする場合、時間単位で時間を入力します。
    -   **のバッチの仮想マシンの初期レプリケーションの時間数を完了する必要があります** と **初期レプリケーションのバッチごとの仮想マシンの数** 初期レプリケーションの要件の計算に使用される設定を入力します。  Site Recovery をデプロイする際には、初期データ セット全体をアップロードする必要があります。 

    ![入力](./media/site-recovery-capacity-planner/inputs.png)

2.  ソース環境の値を入力した後には、以下などが出力されます。

    - **デルタ レプリケーションに必要な帯域幅** (MB/秒)。 1 日の平均データ変更率に基づいてデルタ レプリケーションのネットワーク帯域幅が計算されます。
    - **初期レプリケーションに必要な帯域幅** (MB/秒)。 入力した初期レプリケーションの値に基づいて初期レプリケーションのネットワーク帯域幅が計算されます。 
    - **(Gb 単位) で必要なストレージ** が必要となる Azure ストレージの合計です。
    - **標準ストレージ アカウントの IOPS の合計** 8 K IOPS unit の合計の標準的なストレージ アカウントのサイズに基づいて計算されます。  クイック プランナーの場合、この数値はすべてのソース VM ディスクおよび 1 日のデータ変更率に基づいて計算されます。 詳細なプランナーの場合、この数値は、Standard の Azure VM にマップされている VM 数の合計およびそれらの VM でのデータ変更率に基づいて計算されます。 
    - **標準ストレージ アカウントの数** Vm を保護するために必要な標準的なストレージ アカウントの合計数を提供します。 Standard Storage アカウントの場合、Standard Storage のすべての VM で最大 20000 の IOPS まで対応でき、またディスクごとでは最大 500 の IOPS まで対応できます。 
    - **必要な blob ディスク数** Azure ストレージに作成されるディスクの数を示します。
    - **Premium storage アカウントが必要な数** Vm を保護するために必要な premium storage アカウントの合計数を提供します。 Premium storage アカウントをソース (20000 より大きい) iop の高い VM が必要があることに注意してください。 Premium Storage アカウントでは、最大 80000 の IOPS に対応できます。
    - **Premium storage での IOPS の合計** 256 K IOPS unit の合計の premium storage アカウントのサイズに基づいて計算されます。  クイック プランナーの場合、この数値はすべてのソース VM ディスクおよび 1 日のデータ変更率に基づいて計算されます。 詳細なプランナーの場合、この数値は、Premium の Azure VM (DS および GS シリーズ) にマップされている VM 数の合計およびそれらの VM でのデータ変更率に基づいて計算されます。 
    - **必要な構成サーバーの数** (1) の展開に必要な構成サーバーの数を示します
    - **必要なその他のプロセス サーバーの数** その他のプロセス サーバーは、既定では、構成サーバーに構成されているプロセスのサーバーだけでなく必要かどうかを示しています。
    - **ソースの追加のストレージを 100%** ソースの場所に追加の記憶域が必要かどうかを示しています。
            
    ![出力](./media/site-recovery-capacity-planner/output.png)
 
## 詳細なプランナーの実行


1.  ダウンロードして開きます、 [Azure サイト回復 Capacity Planner](http://aka.ms/asr-capacity-planner-excel) ツールです。 マクロを実行する必要があるので、求められたら編集の有効化とコンテンツの有効化を選択します。 
2.   **プランナーの種類を選択して** 選択 **詳細プランナー** リスト ボックスからです。

    ![Getting Started (概要)](./media/site-recovery-capacity-planner/getting-started-2.png)

3.   **ワークロード認定** ワークシートは、必要な情報を入力します。 マークが付いているフィールドはすべて入力する必要があります。

    -  **プロセッサ コア** 移行元サーバー上のコアの合計数を指定します。
    -  **MB のメモリ割り当て** 移行元サーバーの RAM サイズを指定します。 
    -  **Nic の数** 移行元サーバー上のネットワーク アダプターの数を指定します。 
    -   **合計 (GB) でストレージ** VM ストレージの合計サイズを指定します。 たとえば、ソース サーバーにそれぞれ 500 GB のディスクが 3 つある場合、ストレージの合計サイズは 1500 GB になります。
    -   **接続されているディスクの数** 移行元サーバーのディスクの合計数を指定します。
    -   **ディスクの容量使用率** の平均使用率を指定します。
    -   **毎日の変更率 (%)** 日ごとのデータ ソース サーバーの速度を変更するように指定します。
    -   **マッピング Azure サイズ** をマップする Azure VM のサイズを入力するか。 手動で設定したくない場合はクリックして、**IaaS Vm の計算**します。 手動で設定を入力し、[IaaS Vm を計算する] をクリックすると、最も一致する Azure VM のサイズが計算プロセスによって自動的に特定されるため、手動の設定が上書きされることがあります。

    ![Workload Qualification](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  クリックすると **IaaS Vm の計算** 内容を次に示します。

    - 必須の入力が検証されます。
    - IOPS が計算され、Azure へのレプリケーション対象である各 VM に最も一致する Azure VM のサイズが提案されます。 Azure VM の適切なサイズを検出できない場合、エラーが発行されます。 たとえば、接続されているディスク数が 65 の場合、Azure VM の最大サイズは 64 であるため、エラーが発行されます。
    - Azure VM に使用できるストレージ アカウントが推奨されます。
    - ワークロードに必要な Standard Storage アカウントおよび Premium Storage アカウントの総数が算出されます。 右下にスクロールして、ソース サーバーに使用できる Azure のストレージ タイプとストレージ アカウントを表示します。
    - VM に割り当てられた (Standard または Premium の) 必要なストレージの種類に基づき、残りのテーブルが完了およびソートされます。 Azure のバックアップ要件を満たすすべての VM では、列 A (対象 VM) に [はい] が表示されます。 VM を Azure にバックアップできない場合は、エラーが表示されます。

AA から AE の列が出力され、各 VM の情報が示されます。

![Workload Qualification](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### 例
例として、テーブルに示した値を持つ 6 つの VM に最も一致する Azure VM および Azure ストレージ要件が、ツールによって計算されて割り当てられます。

![Workload Qualification](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- 出力例で以下に注意してください。
    
    - 最初の列は、VM、ディスク、およびチャーンの検証列です。
    - 5 つの VM に Standard Storage アカウントが 2 つと Premium Storage アカウントが 1 つ必要でした。 
    -  1 つ以上のディスクが 1 TB 以上であるために、VM3 は保護の対象とはなりません。
    -  VM1 と VM2 は、最初の Standard Storage アカウントを使用できます。
    -  VM4 は、2 つ目の Standard Storage アカウントを使用できます。
    -  VM5 と VM6 には、Premium Storage アカウントが必要で、いずれも単一のアカウントを使用できます。

    >[AZURE.NOTE]  ディスク レベルではなく、VM レベルでは、標準、およびプレミアムの記憶域の IOPS が計算されます。 標準的な仮想マシンはディスクあたり最大 500 IOPS を処理できます。 ディスクの IOPS が 500 より大きい場合は、Premium Storage が必要になります。 ただし、ディスクの IOPS が 500 を超えても VM ディスクの合計の IOPS がサポートされる標準 Azure VM の制限 (VM サイズ、ディスク数、アダプター数、CPU、メモリ) 内にある場合、プランナーは DS または GS シリーズではなく標準の VM を選択します。 ユーザーは適切な DS または GS シリーズ VM を使用して Azure サイズ セルのマッピングを手動で更新する必要があります。

5. すべての詳細を定義したら、クリックして **計画ツールにデータを送信** を開くには、 **Capacity Planner** かどうか、条件を満たしている保護のためかどうかを表示するワークロードが強調表示されます。


### Capacity Planner でのデータの送信

1.  開くと、 **Capacity Planner** 指定した設定に基づいて、ワークシートが表示されます。 「ワークロード」は、word、 **インフラの入力ソース** 結果が、入力セル **ワークロード認定** ワークシートです。 
2.  変更する場合を変更する必要があります、 **ワークロード認定** ワークシートとをクリックしてデータを送信するプランナーがもう一度ツールです。  

    ![Capacity Planner](./media/site-recovery-capacity-planner/capacity-planner.png)



