<properties
 pageTitle="予測的なメンテナンスの構成済みソリューション | Microsoft Azure"
 description="Azure IoT の予測的なメンテナンスの構成済みソリューションの説明。"
 services=""
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="na"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="12/01/2015"
 ms.author="stevehob"/>

# 予測的なメンテナンスの構成済みソリューションの概要

 *予測メンテナンス* 構成済みのソリューションは、のいずれか、 [ソリューションを構成済み][lnk_preconfigured_solutions] の一部としてリリース [Microsoft Azure の IoT Suite][lnk_iot_suite]します。 このソリューションを使用して作成、予測モデルとリアルタイムのデバイスのテレメトリの収集を統合する [Azure Machine Learning][lnk_machine_learning]します。


Azure IoT Suite を使用することで、企業はすばやく簡単な接続、資産の監視、およびリアルタイムでのデータ分析を実行できるようになります。 予測的なメンテナンスの構成済みのソリューションは、そのデータを取り込んで、豊富なダッシュ ボードと視覚エフェクトを使用して、効率化を推進し、収益ストリームを強化する新しいインテリジェンスをビジネスに提供します。

## シナリオ

Fabrikam は、競争力のある価格での優れたカスタマー エクスペリエンスの提供に取り組んでいる地域航空会社です。 フライトの遅延の原因の 1 つは、メンテナンスです。また、航空機のエンジンのメンテナンスは特に困難です。 飛行中のエンジンの故障は、いかなる代価を払っても避ける必要があります。そのため、Fabrikam では定期的にエンジンを検査し、予定されたメンテナンス プログラムを確実に実行しています。 ただし、航空機のエンジンがすべて同じように損耗するわけではありません。 必要のないメンテナンスも一部、エンジンに対し実行されています。 さらに重要なことは、問題が発生すると、メンテナンスが実行されるまでは、航空機は飛行禁止になるということです。 このため、適切な技術者がいない、または予備の部品を使用できない場所に航空機がある場合、特に費用のかかる遅延が発生します。

Fabrikam の航空機のエンジンは、飛行中のエンジンの状態を監視するセンサーを搭載しています。 Fabrikam では、飛行中に集められたセンサー データの収集に Azure IoT Suite を使用しています。 長年にわたり、エンジンの運用データと障害データを蓄積した後、Fabrikam のデータ サイエンティストは、航空機エンジンの残存耐用年数 (RUL) を予測する方法をモデル化しました。 データ サイエンティストが特定したのは、4 つのエンジン センサーからのデータと障害につながるエンジンの損耗との相関関係です。 Fabrikam は、安全性を確保するための通常の検査を継続しつつ、このモデルにより、飛行中にエンジンから収集されたテレメトリを使用して、毎回の飛行後にそれぞれのエンジンの RUL を計算できるようになりました。 そのため、Fabrikam は、将来における障害の時期を予測し、事前にメンテナンスおよび修復のための計画を立てることができるようになりました。

メンテナンスが必要な時期を予測することで、Fabrikam は、運用状況を最適化し、コストを削減することができます。 メンテナンス コーディネーターは、スケジュール管理者と連携し、特定の場所での航空機の休止に合うようにメンテナンスを計画し、スケジュールの中断を発生させることなく、航空機の運転休止のための十分な時間を確保します。 Fabrikam は上述の計画に応じて技術者のスケジュールを決定し、待機時間なしで効率的に航空機の点検が実行されるようにできます。 在庫管理マネージャーは、メンテナンスの計画を受け取ることで、注文処理と予備部品の在庫を最適化できます。 これにより、Fabrikam は航空機の飛行禁止時間を最小限に抑え、乗客と乗組員の安全を確保しながら、運用コストを削減することができます。

## 予測的なメンテナンス ソリューションの構築方法

理解しておく方法 [Azure IoT スイート][lnk_iot_suite] により、機能のお客様は、予測のメンテナンスの潜在能力を実現するには、これを確認してください必要 [のインフォ グラフィック][lnk_infographic]します。

IoT Suite サービスを通じて収集されたデバイスのテレメトリを使用するこれらの機能を示し、Web アプリケーションのユーザーに見えるようにするために、ソリューションでは、テンプレートとして利用可能な既存の Azure Machine Learning モデルを使用しています。 Microsoft が構築された、 [回帰モデル][lnk_regression_model] 完全なテンプレート、データを公開および<sup>\[1\]</sup>, 、およびステップ バイ ステップ ガイドです。

Azure IoT の予測的なメンテナンスの構成済みのソリューションでは、このテンプレートから作成された回帰モデルを使用しています。このモデルは、Azure サブスクリプションにデプロイされ、発行された API を介して公開されます。 ソリューションには、トレーニング済みのモデルから正確な結果を提供する、4 つのエンジン (合計 100) および 4 つのセンサー (合計 21) のデータ ストリームを示すテスト データのサブセットが含まれます。

*\[1\] A. Saxena と K. Goebel (2008 年)。 「ターボファン エンジンのパフォーマンス低下のシミュレーション データ セット」、NASA だろう Prognostics データ リポジトリ (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/)、NASA だろう研究センター、カリフォルニア州 Moffett フィールド*

## 次のステップ

Azure の IoT が予測のメンテナンスのシナリオを実現する方法に関する詳細についてを参照してください [モ ノのインターネットからの値をキャプチャ][lnk_capture_value]します。

 [チュートリアル][lnk-predictive-walkthrough] 予測の保守の構成済みのソリューションです。

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/en-us/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
