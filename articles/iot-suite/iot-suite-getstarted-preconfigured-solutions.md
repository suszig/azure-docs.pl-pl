<properties
    pageTitle="構成済みソリューションの使用 | Microsoft Azure"
    description="このチュートリアルに従って、Azure IoT Suite 事前構成済みソリューションのデプロイ方法について学習します。"
    services=""
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="na"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="12/03/2015"
     ms.author="dobett"/>

# チュートリアル: IoT 事前構成済みソリューションの使用

## はじめに

Azure の IoT スイート [ソリューションを構成済み][lnk-preconfigured-solutions] IoT の一般的なビジネス シナリオを実装するエンド ツー エンド ソリューションを提供する複数の Azure の IoT サービスを結合します。

このチュートリアルには、プロビジョニングする方法がでは、 *リモート監視* 構成済みのソリューションです。 また、リモート監視の事前構成済みソリューションの基本機能についても詳しく説明します。

このチュートリアルを完了するには、アクティブな Azure サブスクリプションが必要です。

> [AZURE.NOTE]  アカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、[Azure の無料試用版サイト][lnk_free_trial]をご覧ください。

## リモート監視の事前構成済みソリューションのプロビジョニング

1.  ログオン [azureiotsuite.com][lnk-azureiotsuite] アカウントの資格情報、およびをクリックして、Azure を使用して **+** 新しいソリューションを作成します。

2.  をクリックして **選択** 上、 **リモート監視** を並べて表示します。

3.  入力、 **ソリューション名** リモート監視ソリューションを構成済みのです。

4.  選択、 **地域** と **サブスクリプション** ソリューションのプロビジョニングに使用します。

5.  クリックして **ソリューションを作成する** 、プロビジョニング プロセスを開始します。 通常、プロセスの実行までに数分かかります。

## リモート監視ソリューションのダッシュボードの表示

1.  ときに、プロビジョニングが完了し、構成済みのソリューションのタイルを示します **準備**, 、] をクリックして **起動** を新しいタブで、リモートの監視ソリューションのポータルを開きます。

    ![][img-launch-solution]

2.  既定では、ソリューションのポータルを示しています、 *ソリューションのダッシュ ボード*します。 左側のメニューを使用して、他のビューを選択できます。

    ![][img-dashboard]

## ソリューションのデバイス一覧の表示

1.  をクリックして **デバイス** に表示される左側のメニューで、 *デバイスの一覧* このソリューションにします。

    ![][img-devicelist]

2.  プロビジョニング プロセスで作成された 4 つのシミュレートされたデバイスが表示されています。

3.  デバイス一覧で任意のデバイスをクリックすると、デバイスの詳細が表示されます。

    ![][img-devicedetails]

## デバイスへのコマンドの送信

1.  クリックして **コマンド** 、選択したデバイスのデバイスの詳細ウィンドウにします。

    ![][img-devicecommands]

2.  選択 **PingDevice** コマンドの一覧からです。

3.  クリックして **コマンドを送信**します。

4.  [コマンドの履歴] にコマンドの状態が表示されます。

    ![][img-pingcommand]

## 新しいシミュレートされたデバイスの追加

1.  デバイス一覧に戻ります。

2.  クリックして **+ A デバイスの追加** 新しいデバイスを追加するには、左下隅にします。

    ![][img-adddevice]

3.  をクリックして **新規追加** 上、 **デバイスのシミュレートされた** を並べて表示します。

    ![][img-addnew]

4.  選択 **自分で自分のデバイス ID を定義します**, 、一意のデバイス ID の名前を入力および **mydevice_01**します。

5.  クリックして **作成**します。

    ![][img-definedevice]

6. ステップ 3 で **シミュレートされたデバイスを追加する** ] をクリックして **実行** デバイス一覧に戻るにします。

7.  デバイスを表示する **を実行している** デバイスの一覧です。

    ![][img-runningnew]

## ソリューションの規則の表示と編集

1.  ソリューションのダッシュ ボードとビューに戻り、 **アラーム履歴** を並べて表示します。

    ![][img-alarmhistory]

2.  ルール **AlarmTemp** これらのアラームがトリガーされます。

3.  クリックして **ルール** このソリューションのルールを表示するのには、左側のメニューにします。

    ![][img-rules]

4.  この構成済みソリューションでは、2 つの規則がプロビジョニングされます。

5.  クリックして、 **温度** 規則のプロパティを表示するルール一覧内のルール。

6.  をクリックして **編集** ルールのプロパティ] ウィンドウにします。

    ![][img-displayrule]

7.  変更、 **しきい値** 30 にし、その他のすべてのプロパティを同じにしてください。

8.  クリックして **ルールを保存して**します。

    ![][img-editrule]

9.  戻り、 **アラーム履歴** テーブルに、 **ソリューションのダッシュ ボード** し、更新された規則の結果の動作の変更を確認します。

    ![][img-newhistory]

## 次のステップ

これで、機能する事前構成済みソリューションを構築しましたので、次のシナリオに進むことができます。

-   [事前構成済みソリューションのカスタマイズに関するガイダンス][lnk-customize]
-   [予測的なメンテナンスの構成済みソリューションの概要][lnk-predictive]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-alarmhistory]: media/iot-suite-getstarted-preconfigured-solutions/alarmhistory.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-displayrule]: media/iot-suite-getstarted-preconfigured-solutions/displayrule.png
[img-editrule]: media/iot-suite-getstarted-preconfigured-solutions/editrule.png
[img-newhistory]: media/iot-suite-getstarted-preconfigured-solutions/newhistory.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-predictive]: iot-suite-predictive-overview.md

