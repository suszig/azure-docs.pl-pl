<!--author=alkohli last changed: 12/1/15-->


#### 新しいサービスを作成するには

1. Microsoft アカウント資格情報を次の URL から Azure の旧ポータルにログオンを使用して: [http://azure.microsoft.com/](http://azure.microsoft.com/)します。

2. Azure クラシック ポータルで、クリックして **新規** > **Data Services** > **StorSimple Manager** > **簡易作成**します。

3. 表示されるフォームで、次の手順を実行します。
  1. サービスの一意の **名前** サービス用です。 これは、サービスの識別に使用できるフレンドリ名です。 名前の長さは 2 ～ 50 文字とし、文字、数字、ハイフンを含めることができます。 名前の最初と最後は、文字か数字とする必要があります。
  2. 指定、 **場所** サービス用です。 一般的に、デバイスをデプロイする地理的リージョンに最も近い場所を選択します。 あるいは次も考慮できます。 
     
        - Azure 内の既存のワークロードを StorSimple デバイスにもデプロイする場合、そのデータセンターを使用する必要があります。
        - StorSimple Manager サービスと Azure Storage は 2 つの別々の場所に置くことができます。 その場合、StorSimple Manager と Azure Storage のアカウントを別々に作成する必要があります。 Azure ストレージ アカウントを作成するには、Azure 旧ポータルで Azure ストレージ サービスに移動し、手順に従います [Azure ストレージ アカウントの作成](storage-create-storage-account.md#create-a-storage-account)します。 このアカウントを作成した後に追加、StorSimple Manager サービスでの手順に従って [、サービスの新しいストレージ アカウントを構成する](storsimple-deployment-walkthrough.md#configure-a-new-storage-account-for-the-service)です。
         
  3. 選択、 **サブスクリプション** ドロップ ダウン リストからです。 サブスクリプションは、課金アカウントにリンクされます。 このフィールドは、保有するサブスクリプションが 1 つだけの場合は表示されません。
  4. 選択 **新しいストレージ アカウントの作成** サービスとストレージ アカウントを自動的に作成します。 このストレージ アカウントは、"storsimplebwv8c6dcnf" などの特別な名前になります。 別の場所でデータが必要になる場合、このボックスをオフにします。 
  5. クリックして **StorSimple Manager の作成** サービスを作成します。

   ![StorSimple Manager の作成](./media/storsimple-create-new-service/HCS_CreateAService-include.png)

  表示される、 **サービス** ランディング ページです。 サービスの作成には数分かかります。 サービスが正常に作成し、適切に通知され、サービスの状態が変わります **Active**します。
 
   ![サービスの作成](./media/storsimple-create-new-service/HCS_StorSimpleManagerServicePage-include.png)

![使用可能なビデオ](./media/storsimple-create-new-service/Video_icon.png) **ビデオ**

新しい StorSimple Manager サービスを作成する方法のデモ ビデオを視聴するにはクリックして [ここ](http://azure.microsoft.com/documentation/videos/create-a-storsimple-manager-service/)します。
