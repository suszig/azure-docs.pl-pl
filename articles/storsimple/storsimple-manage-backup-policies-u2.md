<properties 
   pageTitle="StorSimple バックアップ ポリシーの管理 | Microsoft Azure"
   description="StorSimple Manager サービスを使用して、手動バックアップ、バックアップのスケジュールを作成して、バックアップのリテンション期間を管理する方法について説明します。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="12/14/2015"
   ms.author="v-sharos"/>

# StorSimple Manager サービスを使用してバックアップ ポリシーを管理する

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## 概要

このチュートリアルは、StorSimple Manager サービスを使用する方法を説明 **バックアップ ポリシー** バックアップ プロセスと、StorSimple ボリュームのバックアップ保有期間を制御するページです。 また、手動バックアップを完了する方法についても説明します。

ボリュームをバックアップする場合は、ローカル スナップショットとクラウド スナップショットのどちらを作成するかを選択できます。 ローカル固定ボリュームをバックアップする場合は、クラウド スナップショットを指定することをお勧めします。 離反要因が多いデータ セットに対して多数のローカル スナップショットを作成すると、ローカル領域が急激に減少する状態が発生します。 ローカル スナップショットの作成を選択した場合は、最新の状態をバックアップするための毎日のスナップショットの作成回数を少なくし、それらを 1 日間保持した後で削除することをお勧めします。

ローカル固定ボリュームのクラウド スナップショットを作成すると、変更されたデータのみがクラウドにコピーされ、クラウドで重複除去と圧縮が行われます。 

## [バックアップ ポリシー] ページ

 **バックアップ ポリシー** ] ページでは、バックアップ ポリシーを管理し、ローカルのスケジュールを設定およびクラウド スナップショットことができます。 (バックアップ ポリシーは、ボリュームのコレクションに対するバックアップのスケジュールとバックアップのリテンション期間の構成に使用します)。バックアップ ポリシーを使用すると、複数のボリュームのスナップショットを同時に取得できます。 これは、バックアップ ポリシーによって作成されたバックアップがクラッシュ整合コピーになることを意味します。  **バックアップ ポリシー** ページには、バックアップ ポリシー、その型、関連付けられているボリューム、保持されている場合、バックアップの数、およびこれらのポリシーを有効にするオプションが一覧表示します。

 **バックアップ ポリシー** ページもでは、次のフィールドの 1 つ以上の既存のバックアップ ポリシーをフィルター処理します。

- **ポリシー名** – ポリシーに関連付けられている名前。 次の種類のポリシーがあります。

   - スケジュールされたポリシー。ユーザーが明示的に作成します。
   - 自動ポリシー。ボリュームの作成時にこのボリュームのオプションの既定のバックアップが有効になっている場合に作成されます。 これらのポリシーの名前は *VolumeName*_Default 場所 *VolumeName* Azure クラシック ポータルでユーザーが構成される StorSimple ボリュームの名前を示します。 自動ポリシーにより、デバイス時刻の 22 時 30分を起点として、1 日のクラウド スナップショットを取得します。
   - インポートされたポリシー。作成元は StorSimple Snapshot Manager です。 このポリシーには、インポート元の StorSimple Snapshot Manager ホストについて説明するタグが付いています。

- **ボリューム** – ポリシーに関連付けられているボリューム。 バックアップ ポリシーに関連付けられているボリュームは、すべてバックアップの作成時にグループ化されます。

- **最終正常バックアップ** – このポリシーで取得した最後の正常なバックアップの日時です。

- **次のバックアップ** – このポリシーによって開始される次回のバックアップの日時です。

- **スケジュール** – バックアップ ポリシーに関連付けられているスケジュールの数。

このページから実行できる、頻繁に使用される操作は次のとおりです。

- バックアップ ポリシーの追加 
- スケジュールの追加または変更 
- バックアップ ポリシーの削除 
- 手動バックアップの取得 
- 複数のボリュームとスケジュールによるカスタム バックアップ ポリシーの作成 

## バックアップ ポリシーの追加

自動的にバックアップをスケジュールするためのバックアップ ポリシーを追加します。 StorSimple デバイスのバックアップ ポリシーを追加するには、Azure クラシック ポータルで次の手順を実行します。 ポリシーを追加した後は、スケジュールを定義することができます (を参照してください [追加のスケジュールを変更または](#add-or-modify-a-schedule))。

[AZURE.INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![使用可能なビデオ](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **ビデオ**

ローカルの作成や、バックアップ ポリシーをクラウドにする方法を説明するビデオを見るには、次のようにクリックします。 [ここ](http://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/)します。


## スケジュールの追加または変更

StorSimple デバイスで、既存のバックアップ ポリシーに関連付けられるスケジュールを追加または変更できます。 スケジュールを追加または変更するには、Azure クラシック ポータルで次の手順を実行します。

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## バックアップ ポリシーの削除

StorSimple デバイスからバックアップ ポリシーを削除するには、Azure クラシック ポータルで次の手順を実行します。

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## 手動バックアップの取得

1 つのボリュームに対し、オンデマンドの (手動) バックアップを作成するには、Azure クラシック ポータルで次の手順を実行します。

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## 複数のボリュームとスケジュールによるカスタム バックアップ ポリシーの作成

複数のボリュームとスケジュールを含むカスタム バックアップ ポリシーを作成するには、Azure クラシック ポータルで次の手順を実行します。

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]


## 次のステップ

- 詳細について [StorSimple Manager サービスを使用して、StorSimple デバイスを管理する](storsimple-manager-service-administration.md)です。
