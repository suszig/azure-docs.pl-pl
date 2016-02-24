<properties 
   pageTitle="StorSimple Snapshot Manager のバックアップ ポリシー | Microsoft Azure"
   description="StorSimple Snapshot Manager MMC スナップインを使用して、スケジュールされたバックアップを管理するバックアップ ポリシーを作成し、管理する方法について説明します。"
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carolz"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/15/2015"
   ms.author="v-sharos" />

# StorSimple Snapshot Manager を使用したバックアップ ポリシーの作成と管理

## 概要

バックアップ ポリシーでは、ローカルまたはクラウドのボリューム データをバックアップするスケジュールを作成します。 バックアップ ポリシーを作成する際に、アイテム保持ポリシーも指定できます  (最大 64 個のスナップショットを保持できます)。バックアップ ポリシーの詳細については、次を参照してください。 [種類とバックアップ ポリシーのバックアップ](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies)します。

このチュートリアルでは、次の方法について説明します。

- バックアップ ポリシーの作成 
- バックアップ ポリシーの編集 
- バックアップ ポリシーの削除 

## バックアップ ポリシーの作成

新しいバックアップ ポリシーを作成するには、次の手順に従います。

#### バックアップ ポリシーを作成するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。

2.  **スコープ** ] ウィンドウで、右クリックし **バックアップ ポリシー**, 、] をクリック **バックアップ ポリシーの作成**します。

    ![バックアップ ポリシーの作成](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

     **ポリシーを作成する** ] ダイアログ ボックスが表示されます。 

    ![[ポリシーの作成] - [全般] タブ](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3.  **全般** ] タブで、次の情報を入力します。

   1.  **名前** テキスト ボックスで、ポリシーの名前を入力します。

   2. **[ボリューム グループ]** ボックスに、ポリシーに関連付けるボリューム グループの名前を入力します。

   3. いずれかを選択 **ローカル スナップショット** または **クラウド スナップショット**します。

   4. 保持するスナップショットの数を選択します。 選択した場合 **すべて**, 、64 個のスナップショットになります (最大) を保持します。 

4. クリックして、 **スケジュール** ] タブをクリックします。

    ![[ポリシーの作成] - [スケジュール] タブ](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5.  **スケジュール** ] タブで、次の情報を入力します。 

   1. クリックして、 **を有効にする** 、次回のバックアップのスケジュールを設定する] チェック ボックスです。

   2.  **設定**, 、[ **1 回**, 、**毎日**, 、**毎週**, 、または **毎月**します。 

   3.  **開始** テキスト ボックスで、カレンダー アイコンをクリックし、開始日を選択します。

   4.  **の詳細設定**, 、任意の繰り返しスケジュールと終了日を設定することができます。

   5. Click **OK**.

バックアップ ポリシーを作成すると、内に、次の情報が表示されます、 **結果** ウィンドウ。

- **名前** – バックアップ ポリシーの名前。

- **型** – ローカル スナップショットまたはクラウド スナップショット。

- **ボリューム グループ** – ポリシーに関連付けられているボリューム グループ。

- **保有期間** – 保持するスナップショットの数。 最大は 64 です。

- **作成した** -このポリシーが作成された日付。

- **有効になっている** – ポリシーが有効になっているかどうか: **True** ; 有効であることを示します **False** それが有効でないあることを示します。 

## バックアップ ポリシーの編集

既存のバックアップ ポリシーを編集するには、次の手順に従います。

#### バックアップ ポリシーを編集するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。 

2.  **スコープ** ] ウィンドウで、をクリックして、 **バックアップ ポリシー** ノードです。 すべてのバックアップ ポリシーが表示される、 **結果** ウィンドウです。 

3. 編集、およびをクリックするポリシーを右クリックして **編集**します。 

    ![バックアップ ポリシーの編集](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. ときに、 **ポリシーを作成する** ウィンドウが表示されたら、変更内容を入力し、をクリックして **OK**します。 

## バックアップ ポリシーの削除

バックアップ ポリシーを削除するには、次の手順に従います。

#### バックアップ ポリシーを削除するには

1. デスクトップ アイコンをクリックして、StorSimple Snapshot Manager を起動します。 

2.  **スコープ** ] ウィンドウで、をクリックして、 **バックアップ ポリシー** ノードです。 すべてのバックアップ ポリシーが表示される、 **結果** ウィンドウです。 

3. クリックして、削除するバックアップ ポリシーを右クリックして **削除**します。 
e
4. 確認メッセージが表示されたら、クリックして **はい**します。

    ![バックアップ ポリシーの削除の確認](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## 次のステップ

- 学習方法 [StorSimple Snapshot Manager を使用した StorSimple ソリューションの管理](storsimple-snapshot-manager-admin.md)します。
- 学習方法 [StorSimple Snapshot Manager を使用して、バックアップ ジョブを表示および管理](storsimple-snapshot-manager-manage-backup-jobs.md)します。
