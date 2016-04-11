<properties
    pageTitle="Mobile Services を使用したイメージの BLOB ストレージへのアップロード (Android) | Mobile Services"
    description="Mobile Services を使用してイメージを Azure Storage にアップロードし、そのイメージに Android アプリからアクセスする方法を説明します。"
    services="mobile-services"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/05/2015"
    ms.author="ricksal"/>

# Android デバイスから Azure ストレージにイメージをアップロードします。

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[AZURE.INCLUDE [mobile-services-selector-upload-data-blob-storage](../../includes/mobile-services-selector-upload-data-blob-storage.md)]

このトピックでは、Android 向け Azure Mobile Services アプリで Azure Storage にイメージをアップロードできるようにする方法について説明します。

Mobile Services では、SQL Database を使用してデータを保存します。 ただし、Azure Storage にバイナリ ラージ オブジェクト (BLOB) データを格納する方が効率的です。 このチュートリアルでは、Mobile Services クイック スタート アプリで Android のカメラを使って写真を撮り、その画像を Azure Storage にアップロードできるようにします。


## はじめにやるべきこと

このチュートリアルを開始する前に、モバイル サービスのクイック スタートの完了してあります: [Get started with Mobile Services]します。

このチュートリアルには、次のものも必要です。

+  [Azure ストレージ アカウント](../storage-create-storage-account.md)
+ カメラ付きの Android デバイス

## アプリの動作

写真画像をアップロードするプロセスは、複数の手順で構成されます。

- まず写真を撮影し、TodoItem 行を使用して、Azure ストレージの新しいメタデータ フィールドを含む SQL データベースに挿入します。
- 新しいモバイル サービス SQL **挿入** スクリプトが共有アクセス署名 (SAS) の Azure ストレージを確認します。
- このスクリプトにより、SAS と BLOB の URI がクライアントに返されます。
- クライアントは、SAS と BLOB URI を使用して写真をアップロードします。

SAS とは

Azure Storage サービスにデータをアップロードするために必要な資格情報をクライアント アプリ内に保存するのは安全ではありません。 代わりに、これらの資格情報をモバイル サービスに保存し、それらを使用して、新しいイメージをアップロードするためのアクセス許可を付与する SAS (Shared Access Signature) を生成します。 SAS は有効期間が 5 分間の資格情報で、Mobile Services によってクライアント アプリに安全に返されます。 アプリケーションは、この一時的な資格情報を使用してイメージをアップロードします。 詳細については、次を参照してください [共有アクセス署名、第 1 部: SAS モデルについて。](storage-dotnet-shared-access-signature-part-1.md)

## サンプル コード
[ここで](https://github.com/Azure/mobile-services-samples/tree/master/UploadImages) このアプリの完成したクライアント ソース コードの一部です。 このコードを実行するには、このチュートリアルの Mobile Services バックエンドの部分を完了する必要があります。

## Azure クラシック ポータルで登録されている挿入スクリプトを更新する

[AZURE.INCLUDE [mobile-services-configure-blob-storage](../../includes/mobile-services-configure-blob-storage.md)]


## イメージをキャプチャしてアップロードするようにクイック スタート アプリケーションを更新する

### Azure Storage Android クライアント ライブラリを参照する

1. ライブラリへの参照を追加する、 **アプリ** > **build.gradle** ファイルにこの行を追加し、 `dependencies` セクション。

        compile 'com.microsoft.azure.android:azure-storage-android:0.6.0@aar'


2. `minSdkVersion` の値を 15 (カメラの API で必要な値) に変更します。

3. キーを押して、 **Sync Project with Gradle Files** アイコン。

### カメラとストレージのマニフェスト ファイルを更新する

1. アプリでは、この行を追加することで利用可能なカメラを含める必要が指定 **AndroidManifest.xml**:

        <uses-feature android:name="android.hardware.camera"
            android:required="true" />

2. アプリには、この行を追加することで、外部ストレージに書き込み許可が必要です指定 **AndroidManifest.xml**:。

        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    Android の外部記憶域とは限りませんが SD カードに注意してください。 詳細については、次を参照してください。 [ファイルの保存](http://developer.android.com/training/basics/data-storage/files.html)します。

### 新しいユーザー インターフェイスのリソース ファイルを更新する

1. 次のコードを追加することで新しいボタンのタイトルを追加、 **strings.xml** ファイルで、 *値* ディレクトリ。

        <string name="preview_button_text">Take Photo</string>
        <string name="upload_button_text">Upload</string>

2.  **Activity_to_do.xml** ファイルで、 **res = > レイアウト** フォルダーの既存のコードの前にこのコードを追加、 **追加** ] ボタンをクリックします。

         <Button
             android:id="@+id/buttonPreview"
             android:layout_width="120dip"
             android:layout_height="wrap_content"
             android:onClick="takePicture"
             android:text="@string/preview_button_text" />

3. 置き換える、 **追加** button 要素を次のコード。

         <Button
             android:id="@+id/buttonUpload"
             android:layout_width="100dip"
             android:layout_height="wrap_content"
             android:onClick="uploadPhoto"
             android:text="@string/upload_button_text" />


### 写真をキャプチャするためのコードを追加する

1.  **ToDoActivity.java** を作成するには、このコードを追加、 **ファイル** 一意の名前を持つオブジェクト。

        // Create a File object for storing the photo
        private File createImageFile() throws IOException {
            // Create an image file name
            String timeStamp = new SimpleDateFormat("yyyyMMdd_HHmmss").format(new Date());
            String imageFileName = "JPEG_" + timeStamp + "_";
            File storageDir = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_PICTURES);
            File image = File.createTempFile(
                    imageFileName,  /* prefix */
                    ".jpg",         /* suffix */
                    storageDir      /* directory */
            );
            return image;
        }

5. Android のカメラ アプリを起動する次のコードを追加します。 、写真を撮影し、[ok] を見るときに、以下のキーを押して **保存**, 、作成したファイルに格納します。

        // Run an Intent to start up the Android camera
        static final int REQUEST_TAKE_PHOTO = 1;
        public Uri mPhotoFileUri = null;
        public File mPhotoFile = null;

        public void takePicture(View view) {
            Intent takePictureIntent = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
            // Ensure that there's a camera activity to handle the intent
            if (takePictureIntent.resolveActivity(getPackageManager()) != null) {
                // Create the File where the photo should go
                try {
                    mPhotoFile = createImageFile();
                } catch (IOException ex) {
                    // Error occurred while creating the File
                    //
                }
                // Continue only if the File was successfully created
                if (mPhotoFile != null) {
                    mPhotoFileUri = Uri.fromFile(mPhotoFile);
                    takePictureIntent.putExtra(MediaStore.EXTRA_OUTPUT, mPhotoFileUri);
                    startActivityForResult(takePictureIntent, REQUEST_TAKE_PHOTO);
                }
            }
        }

### 写真のファイルを BLOB ストレージにアップロードするコードを追加する


1. 最初にいくつかの新しいメタデータ フィールドを追加、 `ToDoItem` に次のコードを追加することによってオブジェクト **ToDoItem.java**します。

        /**
         *  imageUri - points to location in storage where photo will go
         */
        @com.google.gson.annotations.SerializedName("imageUri")
        private String mImageUri;

        /**
         * Returns the item ImageUri
         */
        public String getImageUri() {
            return mImageUri;
        }

        /**
         * Sets the item ImageUri
         *
         * @param ImageUri
         *            Uri to set
         */
        public final void setImageUri(String ImageUri) {
            mImageUri = ImageUri;
        }

        /**
         * ContainerName - like a directory, holds blobs
         */
        @com.google.gson.annotations.SerializedName("containerName")
        private String mContainerName;

        /**
         * Returns the item ContainerName
         */
        public String getContainerName() {
            return mContainerName;
        }

        /**
         * Sets the item ContainerName
         *
         * @param ContainerName
         *            Uri to set
         */
        public final void setContainerName(String ContainerName) {
            mContainerName = ContainerName;
        }

        /**
         *  ResourceName
         */
        @com.google.gson.annotations.SerializedName("resourceName")
        private String mResourceName;

        /**
         * Returns the item ResourceName
         */
        public String getResourceName() {
            return mResourceName;
        }

        /**
         * Sets the item ResourceName
         *
         * @param ResourceName
         *            Uri to set
         */
        public final void setResourceName(String ResourceName) {
            mResourceName = ResourceName;
        }

        /**
         *  SasQueryString - permission to write to storage
         */
        @com.google.gson.annotations.SerializedName("sasQueryString")
        private String mSasQueryString;

        /**
         * Returns the item SasQueryString
         */
        public String getSasQueryString() {
            return mSasQueryString;
        }

        /**
         * Sets the item SasQueryString
         *
         * @param SasQueryString
         *            Uri to set
         */
        public final void setSasQueryString(String SasQueryString) {
            mSasQueryString = SasQueryString;
        }

2. パラメーターを持たないコンストラクターを次のコードに置き換えます。

        /**
         * ToDoItem constructor
         */
        public ToDoItem() {
            mContainerName = "";
            mResourceName = "";
            mImageUri = "";
            mSasQueryString = "";
        }

3. 複数のパラメーターを持つコンストラクターを次のコードに置き換えます。

        /**
         * Initializes a new ToDoItem
         *
         * @param text
         *            The item text
         * @param id
         *            The item id
         */
        public ToDoItem(String text,
                        String id,
                        String containerName,
                        String resourceName,
                        String imageUri,
                        String sasQueryString) {
            this.setText(text);
            this.setId(id);
            this.setContainerName(containerName);
            this.setResourceName(resourceName);
            this.setImageUri(imageUri);
            this.setSasQueryString(sasQueryString);
        }


4.  **ToDoActivity.java** ファイルで置き換えます、 **addItem** メソッド **ToDoActivity.java**  を次のコードでは、イメージをアップロードします。

        /**
         * Add a new item
         *
         * @param view
         *            The view that originated the call
         */
        public void uploadPhoto(View view) {
            if (mClient == null) {
                return;
            }

            // Create a new item
            final ToDoItem item = new ToDoItem();

            item.setText(mTextNewToDo.getText().toString());
            item.setComplete(false);
            item.setContainerName("todoitemimages");

            // Use a unigue GUID to avoid collisions.
            UUID uuid = UUID.randomUUID();
            String uuidInString = uuid.toString();
            item.setResourceName(uuidInString);

            // Send the item to be inserted. When blob properties are set this
            // generates an SAS in the response.
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                            final ToDoItem entity = addItemInTable(item);

                            // If we have a returned SAS, then upload the blob.
                            if (entity.getSasQueryString() != null) {

                           // Get the URI generated that contains the SAS
                            // and extract the storage credentials.
                            StorageCredentials cred =
                                new StorageCredentialsSharedAccessSignature(entity.getSasQueryString());
                            URI imageUri = new URI(entity.getImageUri());

                            // Upload the new image as a BLOB from a stream.
                            CloudBlockBlob blobFromSASCredential =
                                    new CloudBlockBlob(imageUri, cred);

                            blobFromSASCredential.uploadFromFile(mPhotoFileUri.getPath());
                        }

                        runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                if(!entity.isComplete()){
                                    mAdapter.add(entity);
                                }
                            }
                        });
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };

            runAsyncTask(task);

            mTextNewToDo.setText("");
        }


このコードは、新しい TodoItem を挿入する要求をモバイル サービスに送信します。 応答には、ローカル ストレージから Azure Storage の BLOB にイメージをアップロードするために使用される SAS が含まれます。


## イメージのアップロードをテストする

1. Android Studio で **実行**します。 ダイアログ ボックスで、使用するデバイスを選択します。

2. UI が表示されたら、アプリが付いたテキスト ボックスにテキストを入力するときに **ToDo 項目を追加**します。

3. キーを押して **写真を撮る**します。 カメラ アプリが起動したら、写真を撮影します。 チェック マークを押して写真を受け入れます。

4. キーを押して **アップロード**します。 通常どおり、ToDoItem が一覧に追加されていることを確認します。

5. Azure クラシック ポータルでストレージ アカウントとキーを押してに移動、 **コンテナー** タブをクリックし、一覧で、コンテナーの名前をキーを押します。

6. アップロードされている BLOB ファイルの一覧が表示されます。 いずれかを選択し、キーを押して **ダウンロード**します。

7. アップロードした画像がブラウザー ウィンドウに表示されます。


## <a name="next-steps"> </a>次のステップ

モバイル サービスを Blob サービスと統合することによりイメージを安全にアップロードできました。他のバックエンド サービスおよび統合のトピックを参照してください。

+ [SendGrid を使用したモバイル サービスからの電子メールの送信]

  SendGrid 電子メール サービスを使用して、モバイル サービスに電子メール機能を追加する方法について説明します。 このトピックでは、サーバー側スクリプトを追加し、SendGrid を使用して電子メールを送信する方法を示します。

+ [モバイル サービスでのバックエンド ジョブの計画]

  モバイル サービスのジョブ スケジューラ機能を使用して、定義したスケジュールに従って実行されるサーバー スクリプト コードを定義する方法について説明します。

+ [モバイル サービスのサーバー スクリプト リファレンス]

  サーバー側のタスクを実行するサーバー スクリプトの使用および他の Azure コンポーネントおよび外部リソースとの統合に関するトピックを参照してください。

+ [モバイル サービス .NET の使用方法の概念リファレンス]

  .NET でモバイル サービスを使用する方法について説明します


<!-- Anchors. -->
[Install the Storage Client library]: #install-storage-client
[Update the client app to capture images]: #add-select-images
[Update the insert script to generate an SAS]: #update-scripts
[Upload images to test the app]: #test
[Next Steps]:#next-steps

<!-- Images. -->

[2]: ./media/mobile-services-windows-store-dotnet-upload-data-blob-storage/mobile-add-storage-nuget-package-dotnet.png


<!-- URLs. -->
[Send email from Mobile Services with SendGrid]: store-sendgrid-mobile-services-send-email-scripts.md
[Schedule backend jobs in Mobile Services]: mobile-services-schedule-recurring-tasks.md
[Send push notifications to Windows Store apps using Service Bus from a .NET back-end]: http://go.microsoft.com/fwlink/?LinkId=277073&clcid=0x409
[Mobile Services server script reference]: mobile-services-how-to-use-server-scripts.md
[Get started with Mobile Services]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md

[Azure classic portal]: https://manage.windowsazure.com/
[How To Create a Storage Account]: ../storage-create-storage-account.md
[Azure Storage Client library for Store apps]: http://go.microsoft.com/fwlink/p/?LinkId=276866
[Mobile Services .NET How-to Conceptual Reference]: mobile-services-windows-dotnet-how-to-use-client-library.md
[App settings]: http://msdn.microsoft.com/library/windowsazure/b6bb7d2d-35ae-47eb-a03f-6ee393e170f7


