
ここでは、次の操作を行います。

1. [Maintainer の実行可能ファイルを実行するための準備](#to-prepare-to-run-the-maintainer) します。

2. [孤立した Blob をすぐに削除のコンテンツ データベースとごみ箱を準備する](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs)します。

3. [Maintainer.exe を実行します](#to-run-the-maintainer)します。

4. [コンテンツ データベースとごみ箱の設定を元に戻す](#to-revert-the-content-database-and-recycle-bin-settings)します。

#### Maintainer の実行を準備するには

1. Web フロントエンド サーバーで、SharePoint 2013 管理シェルを管理者として開きます。

2. フォルダーに移動 <boot drive>: \Program Files\Microsoft SQL リモート Blob ストレージ 10.50\Maintainer\ します。

3. **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** の名前を **web.config** に変更します。

4. 使用 `aspnet_regiis pdf connectionStrings` web.config ファイルの暗号化を解除します。

5. 復号化された web.config ファイルの下にある、* *<connectionStrings>* *] ノードの SQL server のインスタンスとコンテンツ データベースの名前の接続文字列を追加します。次の例を参照してください。

    `< 名を追加"RBSMaintainerConnectionWSSContent"connectionString = ="データ ソース = SHRPT13 shrpt13 sql12; Initial Catalog = WSS_Content; Integrated Security = True;アプリケーション名 = & quot;WSS_Content & quot; 用のリモート Blob ストレージ Maintainer"providerName="System.Data.SqlClient"/>`

6. 使用 `aspnet_regiis – pef connectionStrings` web.config ファイルの再暗号化します。

7. web.config の名前を Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config に変更します。

#### 孤立した BLOB をすぐに削除するようにコンテンツ データベースとごみ箱を準備するには

1. SQL Server の SQL Management Studio で、対象のコンテンツ データベースに対して次の更新クエリを実行します。

    `use WSS_Content`
    
    `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
    
    `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`

2. Web フロントエンド サーバーの **[サーバーの全体管理]** で、目的のコンテンツ データベースの **[Web アプリケーションの全般設定]** を編集して、一時的にごみ箱を無効にします。 この操作を実行すると、関連するすべてのサイト コレクションのごみ箱も空になります。 そのためには、**[サーバーの全体管理]**、**[アプリケーション管理]**、**[Web アプリケーション (Web アプリケーションの管理)]**、**[SharePoint - 80]**、**[アプリケーションの全般設定]** の順にクリックします。 **[ごみ箱の状態]** を **[オフ]** に設定します。

    ![Web Application General Settings](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### Maintainer を実行するには

- Web フロントエンド サーバーの SharePoint 2013 管理シェルで、次のように Maintainer を実行します。

      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`

    >[AZURE.NOTE] のみ、 `GarbageCollection` StorSimple のこの時点で操作がサポートされています。 また、Microsoft.Data.SqlRemoteBlobs.Maintainer.exe に対して発行されたパラメーターでは大文字と小文字が区別されることにも注意してください。 

#### コンテンツ データベースとごみ箱の設定を元に戻すには

1. SQL Server の SQL Management Studio で、対象のコンテンツ データベースに対して次の更新クエリを実行します。

      `WSS_Content を使用します。`

      `exec mssqlrbs.rbs_sp_set_config_value 'garbage_collection_time_window'、' days 30'`

      `exec mssqlrbs.rbs_sp_set_config_value 'delete_scan_period'、' days 30'`

      `exec mssqlrbs.rbs_sp_set_config_value 'orphan_scan_period'、' days 30'`

2. Web フロントエンド サーバーの **[サーバーの全体管理]** で、目的のコンテンツ データベースの **[Web アプリケーションの全般設定]** を編集して、ごみ箱を再び有効にします。 そのためには、**[サーバーの全体管理]**、**[アプリケーション管理]**、**[Web アプリケーション (Web アプリケーションの管理)]**、**[SharePoint - 80]**、**[アプリケーションの全般設定]** の順にクリックします。 [ごみ箱の状態] を **[オン]** に設定します。





