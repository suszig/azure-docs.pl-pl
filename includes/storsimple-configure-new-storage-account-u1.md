<!--author=alkohli last changed: 9/17/15-->

#### StorSimple 8000 シリーズ Update 1.0 でストレージ アカウントを追加するには

1. StorSimple Manager サービスのランディング ページでサービスを選択し、ダブルクリックします。 この操作により、 **クイック スタート** ページです。 選択、 **構成** ページです。

2. クリックして **ストレージ アカウントの追加/編集**します。

3.  **ストレージ アカウントの追加/編集** ] ダイアログ ボックスをクリックして **新規追加**します。

4.  **プロバイダー** フィールドで、適切なクラウド サービス プロバイダーを選択します。 サポートされているプロバイダーは、Azure、Amazon S3、Amazon S3 RRS、HP、OpenStack です。 クラウド サービス プロバイダーのストレージ アカウントに関連付けられている資格情報と場所を指定します。 資格情報用に表示されるフィールドは、指定したクラウド サービス プロバイダーに応じて異なります。 
  - クラウド サービス プロバイダーとして Azure を選択した場合の指定、 **名前** となり、プライマリ **アクセス キー** 、Microsoft Azure ストレージ アカウントのです。 Azure アカウントの場合、場所は自動的に設定されます。

        ![Add Azure storage account](./media/storsimple-configure-new-storage-account-u1/AddAzureStorageaccount-include.png)

 - Amazon S3 または Amazon S3 RRS を選択した場合の提供、わかりやすい **ストレージ アカウント名**, 、**アクセス キー**, 、および **シークレット キー**します。 Amazon S3 と Amazon S3 RRS の場合、次の場所がサポートされています。

        - US Standard
        - US West (Oregon)
        - US West (Northern California)
        - EU (Ireland)
        - Asia Pacific (Singapore)
        - Asia Pacific (Sydney)
        - Asia Pacific (Tokyo)
        - South America (Sao Paulo)

        ![Add Amazon storage account](./media/storsimple-configure-new-storage-account-u1/AddAmazonStorageaccount-include.png)
            
 - クラウド サービス プロバイダーとして HP を選択した場合の指定、わかりやすい **ストレージ アカウント名**, 、**テナント ID**, 、**ユーザー名**, 、および **パスワード**します。 HP の場合、次の場所がサポートされています。

        - US East
        - US West
      
        ![Add HP storage account](./media/storsimple-configure-new-storage-account-u1/AddHPStorageaccount-include.png)
            
 - 選択した場合に **Openstack** 、クラウド サービス プロバイダーとして提供、 **ホスト名**, 、**アクセス キー**, 、および **シークレット キー**します。

        > [AZURE.NOTE] For all the cloud service providers, excluding Azure, a friendly name is allowed. You can use different friendly names and create more than one storage account with the same set of credentials.

        ![Add Openstack storage account](./media/storsimple-configure-new-storage-account-u1/AddOpenstackStorageaccount-include.png)

5. 選択 **SSL モードを有効にする** 、デバイスとクラウド間のネットワーク通信をセキュリティで保護されたチャネルを作成します。 クリア、 **SSL モードを有効にする** チェック ボックスをプライベート クラウド内で運用している場合のみです。

      > [AZURE.NOTE] プロバイダーとして HP を使用している場合は、SSL 常に有効にします。
        
6. チェック マーク アイコンをクリックします。 ![チェック マーク アイコン](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png). ストレージ アカウントが正常に作成されると通知が表示されます。

7. 新しく作成されたストレージ アカウントが表示されます、 **構成** ] ページの [ **ストレージ アカウント**します。 クリックして **保存** 新しいストレージ アカウントを保存します。 をクリックして **OK** 確認プロンプトにします。

