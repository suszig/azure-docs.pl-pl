## <a name="add-select-images"></a>キャプチャしてイメージをアップロードするクイック スタート クライアント アプリケーションを更新します。

このセクションからプロジェクトを更新する、[モバイル サービスの開始を取得] チュートリアル写真を撮影して Azure Blob ストレージにアップロードします。 イメージをキャプチャするには、このチュートリアルから [CameraCaptureTask] を使用、 `Microsoft.Phone.Tasks` 名前空間。 このクラスは Windows Phone デバイスのカメラ UI を起動し、写真を撮影して、自動的にそのイメージを Windows Phone デバイスのカメラ ロールに保存します。 イメージをカメラ ロールに保存したくない場合で [PhotoCamera] クラスを使用して、 `Microsoft.Devices` 名前空間代わりにします。

1. Visual Studio のソリューション エクスプローラーで、プロジェクトの下の **[プロパティ]** を展開します。 WMAppManifest.xml ファイルを開くと、 **機能** ] タブをクリックして、カメラを有効にする **id \_cap\_isv\_camera**します。 ファイルを閉じて変更内容を保存します。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-WMAppmanifest-wp8.png)

    これにより、コンピューターに接続されたカメラをアプリケーションで使用できます。 ユーザーは、アプリケーションを最初に実行したときに、カメラのアクセスを許可することを求められます。

2. MainPage.xaml ファイルを開き、**ContentPanel** という名前の **Grid** 要素を次のコードに置き換えます。

        
        <Grid x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="Auto" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="2*" />
                <ColumnDefinition Width="2*" />
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.ColumnSpan="2" Text="Enter some text below, click Capture Image to add a captured image. Then click Save to insert a new TodoItem item into your database" TextWrapping="Wrap" Margin="12"/>
            <TextBox Grid.Row="1" Grid.ColumnSpan="2" Name="TextInput" Text="" />
            <Button Name="ButtonCaptureImage" Grid.Row="2" Click="ButtonCaptureImage_Click">Capture Image</Button>
            <Button Grid.Row ="2" Grid.Column="1" Name="ButtonSave" Click="ButtonSave_Click">Save</Button>
            <TextBlock Grid.Row="3" Grid.ColumnSpan="2" Text="Click refresh below to load the unfinished TodoItems from your database. Use the checkbox to complete and update your TodoItems" TextWrapping="Wrap" Margin="12" />
            <Button Grid.Row="4" Grid.ColumnSpan="2" Name="ButtonRefresh" Click="ButtonRefresh_Click">Refresh</Button>
            <phone:LongListSelector Grid.Row="5" Grid.ColumnSpan="2" Name="ListItems">
                <phone:LongListSelector.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Vertical">
                            <CheckBox Name="CheckBoxComplete" IsChecked="{Binding Complete, Mode=TwoWay}" Checked="CheckBoxComplete_Checked" Content="{Binding Text}" Margin="10,5" VerticalAlignment="Center"/>
                            <Image Name="ImageUpload" Source="{Binding ImageUri, Mode=OneWay}" MaxHeight="150"/>
                        </StackPanel>
                    </DataTemplate>
                </phone:LongListSelector.ItemTemplate>
            </phone:LongListSelector>
        </Grid>

    これは、[CameraCaptureTask] を起動する新しいボタンを追加しにイメージが追加、 **ItemTemplate** され、Blob ストレージ サービスにアップロードされたイメージの URI として、バインディング ソースを設定します。

3. MainPage.xaml.cs プロジェクト ファイルを開き、次の **using** ステートメントを追加します。

        using Microsoft.Phone.Tasks;
        using System.IO;
        using Microsoft.WindowsAzure.Storage.Auth;
        using Microsoft.WindowsAzure.Storage.Blob;

4. MainPage.xaml.cs プロジェクト ファイルで、次のプロパティを追加して TodoItem クラスを更新します。

     [JsonProperty(PropertyName = "containerName")]
     public string ContainerName { get; set; }
    
     [JsonProperty(PropertyName = "resourceName")]
     public string ResourceName { get; set; }
    
     [JsonProperty(PropertyName = "sasQueryString")]
     public string SasQueryString { get; set; }
    
     [JsonProperty(PropertyName = "imageUri")]
     public string ImageUri { get; set; } 

5. MainPage.xaml.cs プロジェクト ファイルで、MainPage クラスを更新します。 [CameraCaptureTask] と、キャプチャしたイメージを参照するストリーム オブジェクトを宣言する次のコードを追加します。

     // Using the CameraCaptureTask to allow the user to capture a todo item image //
     CameraCaptureTask cameraCaptureTask;
    
     // Using a stream reference to upload the image to blob storage.
     Stream imageStream = null;

6. MainPage.xaml.cs プロジェクト ファイルで、MainPage クラスを更新します。 CameraCaptureTask を作成し、Completed イベントのイベント ハンドラーを追加するために、コンストラクターに次のコードを追加します。

     // Constructor
     public MainPage()
     {
         InitializeComponent();
    
         cameraCaptureTask = new CameraCaptureTask();
         cameraCaptureTask.Completed += cameraCaptureTask_Completed;
     }
    
     void cameraCaptureTask_Completed(object sender, PhotoResult e)
     {
         imageStream = e.ChosenPhoto;
     }

7. MainPage.xaml.cs プロジェクト ファイルで、MainPage クラスを更新します。 次のコードを追加すると、カメラ UI が表示され、ユーザーが **[Capture Image]** ボタンをクリックしたときにイメージをキャプチャできるようになります。

        private void ButtonCaptureImage_Click(object sender, RoutedEventArgs e)
        {
            cameraCaptureTask.Show();
        }

8. MainPage.xaml.cs プロジェクト ファイルで、MainPage クラスを更新します。 既存 `InsertTodoItem` メソッドを次のコード。

     private async void InsertTodoItem(TodoItem todoItem)
     {
         string errorString = string.Empty;            
    
         if (imageStream != null)
         {
             // Set blob properties of TodoItem.
             todoItem.ContainerName = "todoitemimages";
             todoItem.ResourceName = Guid.NewGuid().ToString() + ".jpg";
         }                       
    
         // Send the item to be inserted. When blob properties are set this
         // generates an SAS in the response.
         await todoTable.InsertAsync(todoItem);  
    
         // If we have a returned SAS, then upload the blob.
         if (!string.IsNullOrEmpty(todoItem.SasQueryString))
         {
             // Get the URI generated that contains the SAS 
             // and extract the storage credentials.
             StorageCredentials cred = new StorageCredentials(todoItem.SasQueryString);
             var imageUri = new Uri(todoItem.ImageUri);
    
             // Instantiate a Blob store container based on the info in the returned item.
             CloudBlobContainer container = new CloudBlobContainer(
                 new Uri(string.Format("https://{0}/{1}",
                     imageUri.Host, todoItem.ContainerName)), cred);                
    
             // Upload the new image as a BLOB from the stream.
             CloudBlockBlob blobFromSASCredential =
                 container.GetBlockBlobReference(todoItem.ResourceName);
             await blobFromSASCredential.UploadFromStreamAsync(imageStream);
    
             // When you request an SAS at the container-level instead of the blob-level,
             // you are able to upload multiple streams using the same container credentials.
    
             imageStream = null;
         }              
    
         // Add the new item to the collection.
         items.Add(todoItem);
         TextInput.Text = "";
     }

 このコードは、新しい TodoItem を挿入する要求をモバイル サービスに送信します (イメージ ファイル名を含む)。 応答には、SAS (その後、BLOB ストア内のイメージの挿入に使用される) と、データ バインド用のイメージの URI が含まれます。

最後の手順では、アプリケーションをテストし、アップロードが成功するかどうかを検証します。

## <a name="test"></a>アプリ内のイメージのアップロードをテストします。

1. Visual Studio で F5 キーを押すと、エミュレーターまたは対象となる実際のデバイスでアプリケーションをテストできます。

2. テキスト ボックスにテキストを入力し、**[Capture Image]** をクリックします。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-wp8.png)

    これにより、カメラ キャプチャ UI が表示されます。

3. イメージをクリックするか、電話機のスナップショット ボタンを押すと、写真が撮影されます。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-camera-wp8.png)

4. **[accept]** をクリックするとそのイメージが採用され、カメラ UI が終了します。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-camera-accept-wp8.png)

5. **[Save]** をクリックし、新しい項目を挿入してイメージをアップロードします。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-save-wp8.png)

6. 新しい項目がアップロード イメージと共にリスト ビューに表示されます。

    ![](./media/mobile-services-windows-phone-upload-to-blob-storage/mobile-upload-blob-app-view-final-wp8.png)
   >[AZURE.NOTE]イメージは Blob ストレージから自動的にダウンロード サービス、 <code>imageUri</code> 新しい項目のプロパティにバインドする、 <strong>イメージ</strong> コントロールです。



[get started with mobile services]: ../articles/mobile-services-windows-phone-get-started.md 
[cameracapturetask]: http://msdn.microsoft.com/library/windowsphone/develop/microsoft.phone.tasks.cameracapturetask(v=vs.105).aspx 
[photocamera]: http://msdn.microsoft.com/library/windowsphone/develop/microsoft.devices.photocamera(v=vs.105).aspx 

