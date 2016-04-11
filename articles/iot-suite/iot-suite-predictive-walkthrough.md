<properties
 pageTitle="予測的なメンテナンスのチュートリアル | Microsoft Azure"
 description="Azure IoT の予測的なメンテナンスの構成済みソリューションのチュートリアルです。"
 services=""
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="na"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="12/02/2015"
 ms.author="araguila"/>

# 予測的なメンテナンスの構成済みソリューションのチュートリアル

## はじめに

IoT Suite の予測的なメンテナンスの構成済みソリューションは、障害が発生する可能性があるポイントを予測するビジネス シナリオに対応したエンド ツー エンド ソリューションです。 この構成済みソリューションを使用すると、メンテナンスの最適化などのアクティビティを先手を打って実行できます。 ソリューションを含む、主要な Azure の IoT Suite サービスを組み合わせて、 [Azure Machine Learning][lnk_machine_learning] パブリック サンプル データ セットに基づいて、航空機のエンジンの残り便利なライフ (RUL) を予測する実験を備えたワークスペース。 このソリューションでは、独自の固有のビジネス要件で必要なこの種類の IoT ソリューションを計画および実装するための開始地点として使用できる、完全なビジネス シナリオが実装されています。

## 論理アーキテクチャ

次の図は、事前構成済みソリューションの論理コンポーネントの概要を示したものです。

![][img-architecture]

青色の項目は、構成済みのソリューションをプロビジョニングするために選択した場所に、プロビジョニングされている Azure サービスです。 構成済みのソリューションは、米国東部、北ヨーロッパまたは東アジア リージョンのいずれかにプロビジョニングできます。

構成済みのソリューションをプロビジョニングするリージョンによっては、一部のリソースは利用できない場合があります。 図のオレンジ色の項目は、選択したリージョンから最も近い利用可能なリージョン (米国中南部、西ヨーロッパや東南アジア) にプロビジョニングされている Azure サービスを示します。

緑色の項目は、シミュレートされている航空機エンジン デバイスです。 シミュレートされているデバイスの詳細については、以下を参照してください。

灰色の項目を実装するコンポーネントを表す *デバイス管理* 機能します。 予測的なメンテナンスの構成済みソリューションの現在のリリースでは、これらのリソースはプロビジョニングできません。 デバイス管理の詳細についてを参照してください、 [監視ソリューションが事前に構成されているリモート][lnk-remote-monitoring]します。

## シミュレートされたデバイス

構成済みのソリューションでは、航空機エンジン デバイスをシミュレーションしています。 このソリューションでは、1 台の航空機にマッピングされる 2 つのエンジンをプロビジョニングしています。 各エンジンは、Machine Learning モデルがそのエンジンの残存耐用年数 (RUL) を計算するために必要なデータを提供するセンサー 9、センサー 11、センサー 14、およびセンサー 15 の 4 種類のテレメトリを出力します。 シミュレートされたデバイスはそれぞれ、次のテレメトリ メッセージを IoT Hub に送信します。

*数サイクル*します。 サイクルとは、長さが 2 ～ 10 時間に渡る完了しているフライトを示します。フライトの期間中、テレメトリ データは 30 分ごとにキャプチャされます。

*製品利用統計情報*します。 エンジン属性を示すセンサーは 4 つあります。 センサーは総称的に、センサー 9、センサー 11、センサー 14、およびセンサー 15 とラベル付けされています。 これら 4 つのセンサーは、RUL 用に Machine Learning モデルから有用な結果を得るテレメトリを表します。 このモデルは、実際のエンジンのセンサー データなど、公開されているデータ セットから作成されています。 元のデータ セットからモデルの作成方法の詳細については、次を参照してください。、 [Cortana Analytics テンプレート ギャラリーには予測メンテナンス][lnk-cortana-analytics]します。

シミュレートされたデバイスは、IoT Hub から送信された次のコマンドを処理できます。

| コマンド | 説明 |
|---------|-------------|
| StartTelemetry | シミュレーションの状態を制御します。<br/>デバイスのテレメトリ送信を開始します。     |
| StopTelemetry  | シミュレーションの状態を制御します。<br/>テレメトリを送信するデバイスを停止します。 |

IoT Hub は、デバイスのコマンドの受信確認を渡します。

## Azure Stream Analytics ジョブ

**ジョブ: 製品利用統計情報** は 2 つのステートメントを使用して、受信デバイス テレメトリ ストリームで動作します。 1 つ目では、デバイスからのすべてのテレメトリを選択し、Web アプリで視覚化されるデータが格納される Blob Storage に送信します。 2 番目のステートメントを計算平均センサーを選択し、値は 2 分スライディング ウィンドウでこのデータを Event hub に送信する **イベント プロセッサ**します。

## イベント プロセッサ

 **イベント プロセッサ** のサイクルが完了した平均センサーの値は、これらの値をエンジンの RUL を計算する機械学習トレーニング済みモデルを公開する API に渡します。

## Azure Machine Learning

元のデータ セットからモデルの作成方法の詳細については、次を参照してください。、 [Cortana Analytics テンプレート ギャラリーには予測メンテナンス][lnk-cortana-analytics]します。

## チュートリアルの開始

このセクションでは、ソリューションのコンポーネントと想定されるユース ケースについて説明し、例を示します。

### 予測的なメンテナンスのダッシュボード

Web アプリケーションでは、このページは、PowerBI JavaScript コントロールを使用して (を参照してください、 [PowerBI ビジュアル リポジトリ][lnk-powerbi]) を視覚化します。

- Blob Storage 内の Stream Analytics ジョブから出力されたデータ。
- 各航空機エンジンの RUL とサイクル数。

### クラウド ソリューションの動作を確認する

Azure ポータルにアクセスし、指定したソリューション名の付いたリソース グループに移動すると、プロビジョニングされたリソースを確認できます。

![][img-resource-group]

構成済みのソリューションをプロビジョニングすると、Machine Learning ワークスペースへのリンクを含む電子メールを受信します。 この Machine Learning ワークスペースに移動することも、[azureiotsuite.com][lnk-azureiotsuite] 、プロビジョニング済みのソリューションにあるときのページ、 **準備ができて** 状態です。

![][img-machine-learning]

ソリューション ポータルでは、4 つのデバイスがプロビジョニングされているサンプルをご確認いただけます。2 台の航空機それぞれにエンジンが 2 つあり、エンジンごとにセンサーが 4 つのあります。 ソリューション ポータルに最初に移動すると、シミュレーションは停止します。

![][img-simulation-stopped]

クリックして **シミュレーションを開始する** 履歴がダッシュ ボードを作成するには、シミュレーションでは、センサーの履歴、RUL、サイクル、および RUL を確認します。

![][img-simulation-running]

RUL が (デモンストレーション用に選択した任意のしきい値である) 160 未満の場合、ソリューション ポータルには RUL の表示の横に警告記号が示され、図内の航空機エンジンは黄色に色付けされます。 RUL 値は、全体的には一般的な下降傾向を示し、上下する傾向があることが分かります。 これはさまざまな長さのサイクルとモデル精度が含まれていることの結果です。

![][img-simulation-warning]

148 サイクルの完全なシミュレーションの完了には約 35 分かかります。 160 の RUL のしきい値には約 5 分で最初に達し、両エンジンがしきい値に達するのは、約 8 分後です。

148 サイクルの完全なデータセットのシミュレーションが実行され、最終的な RUL とサイクル数が出されます。

いずれかでシミュレーションを停止するポイント、クリックしても **開始シミュレーション** データセットの先頭からのシミュレーションを再生します。

## 次のステップ

参照してください、変更をすることがあります、予測のメンテナンスが構成済みのソリューションを実行したようになりました [構成済みのソリューションをカスタマイズする方法のガイダンス][lnk-customize]します。

  
[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md

