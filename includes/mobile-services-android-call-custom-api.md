
###<a name="update-app"></a>カスタム API を呼び出すようにアプリケーションを更新する

1. "Complete All" というラベルのボタンを既存のボタンの横に追加し、両方のボタンを 1 行下に移動します。 Android Studio で開く、 *res\layout\activity_to_do.xml* クイック スタート プロジェクト内のファイルを探し、 **LinearLayout** を含む要素、 **ボタン** という名前の要素 `buttonAddToDo`します。 コピー、 **LinearLayout** 元の直後に貼り付けます。 削除、 **ボタン** 、最初の要素 **LinearLayout**します。

2. 2 番目の **LinearLayout**, 、削除、 **EditText** 要素、し、既存のすぐ後、次のコードを追加 **ボタン** 要素。 

        <Button
            android:id="@+id/buttonCompleteItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="completeItem"
            android:text="@string/complete_button_text" />

    これにより、ページの別の行にある既存のボタンの横に新しいボタンが追加されます。

3. 2 番目 **LinearLayout** 今すぐ次のようにします。

         <LinearLayout
            android:layout_width="match_parent" 
            android:layout_height="wrap_content" 
            android:background="#71BCFA"
            android:padding="6dip"  >
            <Button
                android:id="@+id/buttonAddToDo"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:onClick="addItem"
                android:text="@string/add_button_text" />
            <Button
                android:id="@+id/buttonCompleteItem"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:onClick="completeItem"
                android:text="@string/complete_button_text" />
        </LinearLayout>
    

4. res\values\string.xml ファイルを開き、次のコード行を追加します。

        <string name="complete_button_text">Complete All</string>



5. プロジェクト エクスプ ローラーを右クリックしてでプロジェクト名、 *src* フォルダー (`com.example.{your projects name}`)、選択 **新規** 、 **クラス**します。 ダイアログ ボックスで、次のように入力します。 **MarkAllResult** クラス名] フィールドで [ok] をクリックし、作成されたクラス定義を次のコードに置き換えます。

        import com.google.gson.annotations.SerializedName;
        
        public class MarkAllResult {
            @SerializedName("count")
            public int mCount;
            
            public int getCount() {
                return mCount;
            }
        
            public void setCount(int count) {
                    this.mCount = count;
            }
        }

    このクラスは、カスタム API から返される行数の値を保持する目的で使用します。 

6. 検索、 **refreshItemsFromTable** メソッドで、 **ToDoActivity.java** ファイルを開き、ことを確認して内のコードの最初の行、 `try` ブロックは、次のようになります。

        final MobileServiceList<ToDoItem> result = mToDoTable.where().field("complete").eq(false).execute().get();

    ここでは、完了済みの項目が返されないように、項目をフィルターで処理しています。

7. 確認して **ToDoActivity.java** ファイルの先頭に次のインポートが含まれています。

        import com.google.common.util.concurrent.FutureCallback;
        import com.google.common.util.concurrent.Futures;
        import com.google.common.util.concurrent.ListenableFuture;

8.  **ToDoActivity.java** ファイルに次のメソッドを追加します。

    public void completeItem(View view) {
        
        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll2", MarkAllResult.class ); 
            
            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }
                
                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();    
                }
            });
        }
    
    このメソッドは、処理、 **クリックして** 新しいボタンのイベントです。  **InvokeApi** メソッドを新しいカスタム API に POST 要求を送信すると、クライアントで呼び出されます。 カスタム API から返された結果は、メッセージ ダイアログに表示されます。エラーが発生した場合はそれらも表示されます。

### アプリケーションをテストする

1.  **実行** ] メニューのをクリックして **アプリケーションを実行する** を Android エミュレーターまたは接続された Android デバイスで、プロジェクトを開始します。

    これにより、モバイル サービスから項目を取得するクエリを、クライアント ライブラリを使用して送信するアプリケーションが Android SDK でビルドされ、実行されます。


2. アプリケーションにテキストを入力 **Insert a TodoItem**, 、クリックして **追加**します。

3. 前の手順を繰り返して、複数の Todo 項目をリストに追加します。

4. クリックして、 **Complete All** ] ボタンをクリックします。

    ![](./media/mobile-services-android-call-custom-api/mobile-custom-api-android-completed.png)

    完了としてマークされた項目の数を示すメッセージ ダイアログが表示され、フィルター処理済みのクエリが再度実行されて、すべての項目がリストから消去されます。
