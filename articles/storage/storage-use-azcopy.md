<properties 
    pageTitle="AzCopy を使用してストレージにデータをコピーまたは移動する |Microsoft Azure" 
    description="AzCopy ユーティリティを使用して、BLOB、テーブル、およびファイル間でデータを移動またはコピーします。 ローカル ファイルから Azure ストレージにデータをコピーする、またはストレージ アカウント内またはその間でデータをコピーします。 Azure Storage にデータを簡単に移行します。" 
    services="storage" 
    documentationCenter="" 
    authors="micurd" 
    manager="jahogg" 
    editor="cgronlun"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/10/2015" 
    ms.author="micurd"/>

# AzCopy コマンド ライン ユーティリティを使用してデータを転送する

## 概要

AzCopy は、Microsoft Azure の BLOB、ファイル、およびテーブル ストレージ間でデータをコピーするために設計されたコマンド ライン ユーティリティです。 Azcopy では、簡単なコマンドを使用して最適なパフォーマンスで、ファイル システムと Azure Storage 間でデータを移行できます。 ストレージ アカウント内またはストレージ アカウント間で、1 つのオブジェクトから別のオブジェクトにデータをコピーすることもできます。 

> [AZURE.NOTE] このガイドでは、AzCopy 5.0 以降がインストールされていることを前提としています。

Microsoft Azure ストレージのデータ移動のライブラリのプレビューは経由でダウンロードできるようになりました [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement)します。 Data Movement Library は AzCopy のコア ライブラリです。 ソース コードは [GitHub](https://github.com/Azure/azure-storage-net-data-movement)します。 詳細については、次を参照してください。 [Azure ストレージのデータ移動ライブラリ プレビューの概要](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)します。

## AzCopy のダウンロードとインストール

1. ダウンロード、 [AzCopy の最新バージョン](http://aka.ms/downloadazcopy)
2. インストールを実行します。 既定では、AzCopy は `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy\AzCopy.exe` (64 ビット Windows を実行しているマシンの場合) または `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy\AzCopy.exe` (32 ビット Windows を実行しているマシンの場合) にインストールされます。 ただし、インストール パスはセットアップ ウィザードで変更できます。
3. 必要に応じて、AzCopy のインストール先をシステム パスに追加できます。

## AzCopy コマンド ライン構文の理解

次に、コマンド ウィンドウを開き、`AzCopy.exe` 実行可能ファイルが格納されている、コンピューターの AzCopy インストール ディレクトリに移動します。 AzCopy コマンドの基本構文は次のとおりです。

    AzCopy /Source:<source> /Dest:<destination> /Pattern:<filepattern> [Options]

> [AZURE.NOTE] AzCopy バージョン 3.0.0 より、AzCopy コマンドライン構文必要がパラメーター名を含めてすべてのパラメーターを指定する *例:*, 、`/ParameterName:ParameterValue`です。

## 初めての AzCopy コマンドを書く

AzCopy を試す簡単な方法は、ローカル ファイル システムから BLOB ストレージにファイルをアップロードすることです。 コンソール ウィンドウで、次のコマンドを実行します。リソース名は有効なリソース名に置き換えます。
    
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt

1 つのファイルをコピーする場合は、ファイル名と `/Pattern` オプションを指定する必要があります。 以下のセクションで、その他のサンプルを示します。

## パラメーターの概要

以下の表に、AzCopy 用パラメーターを示します。 AzCopy の使用中にコマンド ラインから以下のコマンドのいずれかを入力して、ヘルプを表示することもできます。

- AzCopy に関する詳細なコマンド ライン ヘルプを表示する場合: `AzCopy /?`
- AzCopy パラメーターに関する詳細なヘルプを表示する場合: `AzCopy /?:SourceKey`
- コマンド ラインの例を表示する場合: `AzCopy /?:Samples` 

<table>
  <tr>
    <th>オプション名</th>
    <th>説明</th>
    <th>Blob Storage への適用の有無 (Y/N)</th>
    <th>File Storage への適用の有無 (Y/N)</th>
    <th>Table Storage への適用の有無 (Y/N)</th>
  </tr>
  <tr>
    <td><b>/Source: & lt; ソース & gt;</b></td>
    <td>コピー元となるソース データを指定します。 ソースには、ファイル システムのディレクトリ、BLOB コンテナー、BLOB 仮想ディレクトリ、ストレージ ファイル共有、ストレージ ファイルのディレクトリ、または Azure テーブルを指定できます。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/Dest: & lt; 宛先 & gt;</b></td>
    <td>コピー先を指定します。 コピー先には、ファイル システムのディレクトリ、BLOB コンテナー、BLOB 仮想ディレクトリ、ストレージ ファイル共有、ストレージ ファイル ディレクトリ、または Azure テーブルを指定できます。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/Pattern: & lt; ファイル パターン & gt;</b></td>
      <td>
          Specifies a file pattern that indicates which files to copy. The behavior of the /Pattern parameter is determined by the location of the source data, and the presence of the recursive mode option. Recursive mode is specified via option /S.
          <br /><br />
          If the specified source is a directory in the file system, then standard wildcards are in effect, and the file pattern provided is matched against files within the directory. If option /S is specified, then AzCopy also matches the specified pattern against all files in any subfolders beneath the directory.
          <br /><br />
          If the specified source is a blob container or virtual directory, then wildcards are not applied. If option&nbsp;/S&nbsp;is specified, then AzCopy interprets the specified file pattern as a blob prefix. If option&nbsp;/S&nbsp;is not specified, then AzCopy matches the file pattern against exact blob names.
          <br /><br />
          If the specified source is an Azure file share, then you must either specify the exact file name, (e.g.&nbsp;abc.txt) to copy a single file, or specify option&nbsp;/S&nbsp;to copy all files in the share recursively. Attempting to specify both a file pattern and option /S&nbsp;together will result in an error.
          <br /><br />
          AzCopy uses case-sensitive matching when the /Source is a blob container or blob virtual directory, and uses case-insensitive matching in all the other cases.
          <br/><br />
          The default file pattern used when no file pattern is specified is *.* for a file system location or an empty prefix for an Azure Storage location. Specifying multiple file patterns is not supported.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/Destkey: & lt; ストレージ キー & gt;</b></td>
    <td>宛先リソースのストレージのアカウント キーを指定します。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td class="auto-style1"><b>/DestSAS: & lt; sas トークン & gt;</b></td>
    <td class="auto-style1">宛先に対して、READ および WRITE アクセス許可を使用する Shared Access Signature (SAS) を指定します (該当する場合)。 特殊なコマンド ライン文字が含まれる可能性があるため、SAS は二重引用符で囲みます。<br /><br />
        If the destination resource is a blob container, file share or table, you can either specify this option followed by the SAS token, or you can specify the SAS as part of the destination blob container, file share or table&#39;s URI, without this option.<br /><br />
        If the source and destination are both blobs, then the destination blob must reside within the same storage account as the source blob.</td>
    <td class="auto-style1">Y</td>
    <td class="auto-style1">Y<br /></td>
    <td class="auto-style1">Y<br /></td>
  </tr>
  <tr>
    <td><b>/Sourcekey: & lt; ストレージ キー & gt;</b></td>
    <td>ソース リソースのストレージのアカウント キーを指定します。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/SourceSAS: & lt; sas トークン & gt;</b></td>
    <td>ソースに対して、READ および LIST アクセス許可を使用する Shared Access Signature を指定します (該当する場合)。 特殊なコマンド ライン文字が含まれる可能性があるため、SAS は二重引用符で囲みます。
        <br /><br />
        If the source resource is a blob container, and neither a key nor a SAS is provided, then the blob container will be read via anonymous access.
        <br /><br />
        If the source is a file share or table, a key or a SAS must be provided.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/S</b></td>
    <td>コピー操作の再帰モードを指定します。 再帰モードでは、AzCopy は指定のファイル パターンに一致するすべての BLOB またはファイル (サブフォルダー内を含む) をコピーします。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/Blobtype: & lt; ブロック |ページ |追加 & gt;</b></td>
    <td>宛先 BLOB がブロック BLOB、ページ BLOB、追加 BLOB のどれであるかを指定します。 <br /><br />
    This option is applicable only when uploading a blob; otherwise, an error is generated. If the destination is a blob and this option is not specified, then by default AzCopy will create a block blob.</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/CheckMD5</b></td>
    <td>ダウンロードするデータの MD5 ハッシュを計算し、BLOB に格納されている MD5 ハッシュまたはファイルの Content-MD5 プロパティが計算したハッシュに一致するかどうかを検証します。 MD5 チェックは既定では無効になっているため、MD5 チェックを実行するにはデータのダウンロード時にこのオプションを指定する必要があります。
    <br /><br />
    Note that Azure Storage doesn't guarantee that the MD5 hash stored for the blob or file is up-to-date. It is client's responsibility to update the MD5 whenever the blob or file is modified.
    <br /><br />
    AzCopy always sets the Content-MD5 property for an Azure blob or file after uploading it to the service.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/Snapshot</b></td>
    <td>スナップショットを転送するかどうかを示します。 このオプションはソースが BLOB の場合のみに有効です。 
        <br /><br />
        The transferred blob snapshots are renamed in this format: [blob-name] (snapshot-time)[extension]. 
        <br /><br />
        By default, snapshots are not copied.</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/V:[verbose log-file]</b></td>
    <td>詳細な状態メッセージをログ ファイルに出力します。
    <br /><br />既定では、詳細なログ ファイルは、 <code>AzCopyVerbose.log</code> という名前で、 <code>%LocalAppData%\Microsoft\Azure\AzCopy</code>. このオプションで既存のファイルの場所を指定すると、そのファイルに詳細なログが追加されます。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/Z:[journal-file-folder]</b></td>
    <td>操作を再開するジャーナル ファイル フォルダーを指定します。<br />
        AzCopy always supports resuming if an operation has been interrupted.<br />
        If this option is not specified, or it is specified without a folder path, then AzCopy will create the journal file in the default location, which is <code>%LocalAppData%\Microsoft\Azure\AzCopy</code>.<br />
        Each time you issue a command to AzCopy, it checks whether a journal file exists in the default folder, or whether it exists in a folder that you specified via this option. If the journal file does not exist in either place, AzCopy treats the operation as new and generates a new journal file.
        <br /><br />
        If the journal file does exist, AzCopy will check whether the command line that you input matches the command line in the journal file. If the two command lines match, AzCopy resumes the incomplete operation. If they do not match, you will be prompted to either overwrite the journal file to start a new operation, or to cancel the current operation. 
        <br /><br />
        The journal file is deleted upon successful completion of the operation.
        <br /><br />
        Note that resuming an operation from a journal file created by a previous version of AzCopy is not supported.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/@:parameter-file</b></td>
    <td>パラメーターを含むファイルを指定します。 AzCopy は、このファイルのパラメーターを、コマンド ラインで指定された場合と同様に処理します。<br /> 
        In a response file, you can either specify multiple parameters on a single line, or specify each parameter on its own line. Note that an individual parameter cannot span multiple lines. 
        <br /><br />
        Response files can include comments lines that begin with the <code>#</code> で始まるコメント行を含めることができます。 
        <br /><br />
        You can specify multiple response files. However, note that AzCopy does not support nested response files.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/Y</b></td>
    <td>AzCopy のすべての確認プロンプトを表示しません。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/L</b></td>
    <td>一覧の操作のみ指定します。データはコピーされません。
    <br /><br />
    AzCopy will interpret the using of this option as a simulation for running the command line without this option /L and count how many objects will be copied, you can specify option /V at the same time to check which objects will be copied in the versbose log.
    <br /><br />
    The behavior of this option is also determined by the location of the source data and the presence of the recursive mode option /S and file pattern option /Pattern.
    <br /><br />
    AzCopy requires LIST and READ permission of this source location when using this option.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/MT</b></td>
    <td>ダウンロードしたファイルの最後の変更時刻をソースの BLOB またはファイルと同じに設定します。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XN</b></td>
    <td>新しいソース リソースを除外します。 ソースが最後に変更された時刻が宛先と同じか、それよりも後である場合、リソースはコピーされません。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XO</b></td>
    <td>古いソース リソースを除外します。 ソースが最後に変更された時刻が宛先と同じか、それよりも前である場合、リソースはコピーされません。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/A</b></td>
    <td>Archive 属性が設定済みのファイルのみアップロードします。</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/IA:[RASHCNETOI]</b></td>
    <td>指定の属性のいずれかが設定されているファイルのみアップロードします。<br /><br />
        Available attributes include:  
        <br /><br />
        R&nbsp;&nbsp;&nbsp;Read-only files
        <br />
        A&nbsp;&nbsp;&nbsp;Files ready for archiving
        <br />
        S&nbsp;&nbsp;&nbsp;System files
        <br />
        H&nbsp;&nbsp;&nbsp;Hidden files
        <br />
        C&nbsp;&nbsp;&nbsp;Compressed file
        <br />
        N&nbsp;&nbsp;&nbsp;Normal files
        <br />
        E&nbsp;&nbsp;&nbsp;Encrypted files
        <br />
        T&nbsp;&nbsp;&nbsp;Temporary files
        <br />
        O&nbsp;&nbsp;&nbsp;Offline files
        <br />
        I&nbsp;&nbsp;&nbsp;Non-indexed Files</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/XA:[RASHCNETOI]</b></td>
    <td>指定の属性のいずれかが設定されているファイルを除外します。<br /><br />
        Available attributes include:  
        <br /><br />
        R&nbsp;&nbsp;&nbsp;Read-only files  
        <br />
        A&nbsp;&nbsp;&nbsp;Files ready for archiving  
        <br />
        S&nbsp;&nbsp;&nbsp;System files  
        <br />
        H&nbsp;&nbsp;&nbsp;Hidden files  
        <br />
        C&nbsp;&nbsp;&nbsp;Compressed file  
        <br />
        N&nbsp;&nbsp;&nbsp;Normal files  
        <br />
        E&nbsp;&nbsp;&nbsp;Encrypted files  
        <br />
        T&nbsp;&nbsp;&nbsp;Temporary files  
        <br />
        O&nbsp;&nbsp;&nbsp;Offline files  
        <br />
        I&nbsp;&nbsp;&nbsp;Non-indexed Files</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/区切り文字: & lt; 区切り記号 & gt;</b></td>
    <td>BLOB 名で仮想ディレクトリを区切るために使用する区切り文字を示します。<br /><br />
        By default, AzCopy uses / as the delimiter character. However, AzCopy supports using any common character (such as @, #, or %) as a delimiter. If you need to include one of these special characters on the command line, enclose the file name with double quotes. 
        <br /><br />
        This option is only applicable for downloading blobs.</td>
    <td>Y</td>
    <td>N</td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/NC: & lt; 数-concurrents の & gt;</b></td>
    <td>同時操作の数を指定します。
        <br /><br />
        AzCopy by default starts a certain number of concurrent operations to increase the data transfer throughput. Note that large number of concurrent operations in a low-bandwidth environment may overwhelm the network connection and prevent the operations from fully completing. Throttle concurrent operations based on actual available network bandwidth.
        <br /><br />
        The upper limit for concurrent operations is 512.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/SourceType:Blob|Table</b></td>
    <td>ストレージ エミュレーターで実行されている、ローカル開発環境で利用可能な BLOB を <code>source セクション</code> リソースに指定します。</td>
    <td>Y</td>
    <td>N</td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/DestType:Blob|Table</b></td>
    <td>ストレージ エミュレーターで実行されている、ローカル開発環境で利用可能な BLOB を <code>destination</code> リソースに指定します。</td>
    <td>Y</td>
    <td>N</td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><strong>/PKRS: & lt; & quot; key1 #key2 #key3 #... & quot; & gt;</strong></td>
    <td>テーブル データを並行してエクスポートできるようにパーティション キーの範囲を分割します。これによって、エクスポート操作の速度が速くなります。
        <br /><br />
        If this option is not specified, then AzCopy uses a single thread to export table entities. For example, if the user specifies /PKRS:&quot;aa#bb&quot;, then AzCopy starts three concurrent operations.
        <br /><br />
        Each operation exports one of three partition key ranges, as shown below: 
        <br /><br />
        &nbsp;&nbsp;&nbsp;&#91;&lt;first partition key&gt;, aa&#41; 
        <br /><br />
        &nbsp;&nbsp;&nbsp;&#91;aa, bb&#41;
        <br /><br />
        &nbsp;&nbsp;&nbsp;&#91;bb, &lt;last partition key&gt;&#93; </td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><strong>/SplitSize:</strong><file-size><strong>& lt; ファイル サイズ & gt;</strong></td>
    <td>エクスポートされるファイルの分割サイズを MB 単位で指定します。許容される最小値は 32 です。
        <br /><br />
        If this option is not specified, AzCopy will export table data to single file.
        <br /><br />
        If the table data is exported to a blob, and the exported file size reaches the 200 GB limit for blob size, then AzCopy will split the exported file, even if this option is not specified. </td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/EntityOperation: & lt; InsertOrSkip |InsertOrMerge |InsertOrReplace & gt;
</b>
</td>
    <td>テーブル データのインポート動作を指定します。
        <br /><br />
        InsertOrSkip - Skips an existing entity or inserts a new entity if it does not exist in the table.
        <br /><br />
        InsertOrMerge - Merges an existing entity or inserts a new entity if it does not exist in the table.
        <br /><br />
        InsertOrReplace - Replaces an existing entity or inserts a new entity if it does not exist in the table. </td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/Manifest: & lt; マニフェスト ファイル & gt;</b></td>
    <td>テーブルのエクスポートおよびインポート操作のマニフェスト ファイルを指定します。
    <br /><br />
    This option is optional during the export operation, AzCopy will generate a manifest file with predefined name if this option is not specified.
    <br /><br />
    This option is required during the import operation for locating the data files.</td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /></td>
  </tr>
  <tr>
    <td><b>/SyncCopy</b></td>
    <td>2 つの Azure ストレージ エンドポイント間で BLOB またはファイルを同期的にコピーするかどうかを示します。 <br /><br />
        AzCopy by default uses server-side asynchronous copy. Specify this option to perform a synchronous copy, which downloads blobs or files to local memory and then uploads them to Azure Storage.
        <br /><br />
        You can use this option when copying files within Blob storage, within File storage, or from Blob storage to File storage or vice versa.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
  <tr>
    <td><b>/SetContentType: & lt; コンテンツ タイプ & gt;</b></td>
    <td>コピー先の BLOB またはファイルの MIME コンテンツの種類を指定します。 <br /><br />
        AzCopy sets the content type for a blob or file to <code>アプリケーションまたはオクテット ストリーム</code> 既定では。 このオプションで値を明示的に指定することで、すべての BLOB またはファイルのコンテンツの種類を設定できます。 
        <br /><br />
        If you specify this option without a value, then AzCopy will set each blob or file's content type according to its file extension.</td>
    <td>Y</td>
    <td>Y<br /></td>
    <td>N</td>
  </tr>
    <tr>
    <td><b>/PayloadFormat: & lt;JSON |CSV & gt;</b></td>
    <td>テーブルのエクスポートされたデータ ファイルの形式を指定します。<br /><br />
    If this option is not specified, by default AzCopy exports table data file in JSON format.</td>
    <td>N</td>
    <td>N</td>
    <td>Y<br /></td>
  </tr>
</table>
<br/>

## データのコピー中の同時書き込みの制限

AzCopy を使用して BLOB またはファイルをコピーする場合は、コピー中に別のアプリケーションがそのデータを変更している可能性があることに注意してください。 可能な場合は、コピーしているデータがコピー操作中に変更されないようにしてください。 たとえば、Azure 仮想マシンに関連付けられている VHD をコピーしている場合は、その VHD に別のアプリケーションが書き込まないようにします。 これを行う適切な方法は、コピーされるリソースをリースすることです。 または、最初に VHD のスナップショットを作成してからそのスナップショットをコピーします。

コピーしている BLOB またはファイルに対する他のアプリケーションによる書き込みを回避できない場合は、ジョブが終了した時点で、コピー対象のリソースがソース リソースとの間に完全なパリティを保持していない可能性があることを覚えておいてください。

## AzCopy を使用した Azure BLOB のコピー

以下の例は、AzCopy を使用して BLOB をコピーするさまざまなシナリオを示したものです。

### 単一の BLOB のコピー

**ファイル システムのファイルを BLOB ストレージにアップロードする場合:**
    
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt

**BLOB ストレージの BLOB をファイル システムにダウンロードする場合:**

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

ストレージ アクセス キーの操作の詳細については、次を参照してください [表示、コピーおよび再生成するストレージ アクセス キー](../storage-create-storage-account/#regeneratestoragekeys)します。

### サーバー側コピーを通じた BLOB のコピー

BLOB を 1 つのストレージ アカウント内または複数のストレージ アカウント間でコピーする場合は、サーバー側でコピー操作が実行されます。 サーバー側コピー操作の詳細については、次を参照してください。 [非同期アカウント間の Copy Blob の概要](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx)します。

**1 つのストレージ アカウント内で BLOB をコピーする場合:**

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt 

**複数のストレージ アカウント間で BLOB をコピーする場合:**

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
 
### セカンダリ リージョンの BLOB のコピー 

ストレージ アカウントで地理冗長ストレージの読み取りアクセスが有効に設定されている場合は、セカンダリ リージョンのデータをコピーできます。 

**セカンダリの BLOB をプライマリ アカウントにコピーする場合:**

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

**セカンダリの BLOB をファイル システムのファイルにダウンロードする場合:**

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

### 新しい BLOB コンテナーまたは仮想ディレクトリへのファイルのアップロード

**新しい BLOB コンテナーにファイルをアップロードする**

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mynewcontainer /DestKey:key /Pattern:abc.txt

存在しない宛先コンテナーを指定すると、AzCopy によってコンテナーが作成され、そのコンテナーにファイルがアップロードされることに注意してください。

**新しい BLOB 仮想ディレクトリにファイルをアップロードする**

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

メモを指定された仮想ディレクトリが存在しない場合アップロードを名前に仮想ディレクトリを含めるファイル (*など*, 、`vd/abc.txt` 上記の例で)。

### 新しいフォルダーへの BLOB のダウンロード

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

フォルダー `C:\myfolder` が存在しない場合、AzCopy によってファイル システム内にフォルダーが作成され、この新しいフォルダーに `abc.txt ` がダウンロードされます。

### ディレクトリ内のファイルおよびサブフォルダーをコンテナーに再帰的にアップロードする

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

オプション `/S` を指定すると、指定したディレクトリの内容が再帰的に、つまり、すべてのサブフォルダーと、それらのサブフォルダーに含まれるファイルも、BLOB ストレージにコピーされます。 たとえば、フォルダー `C:\myfolder` に以下のファイルがあるとします。

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

コピー操作後にコンテナーに含まれるのは以下のファイルです。

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

### ディレクトリからコンテナーにファイルを非再帰的にアップロードする

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key

オプション `/S` をコマンド ラインに指定しない場合は、AzCopy は再帰的にはコピーを実行しません。 指定されたディレクトリ内のファイルのみコピーされ、サブフォルダーとその中のファイルはコピーされません。 たとえば、フォルダー `C:\myfolder` に以下のファイルがあるとします。

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

コピー操作後にコンテナーに含まれるのは以下のファイルです。

    abc.txt
    abc1.txt
    abc2.txt

### コンテナー内のすべての BLOB をファイル システムのディレクトリに再帰的にダウンロードする

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

以下の BLOB が指定されたコンテナーにあるとします。  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

コピー操作後にディレクトリ `C:\myfolder` に含まれるのは以下のファイルです。

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

### 仮想 BLOB の内の BLOB をファイル システムのディレクトリに再帰的にダウンロードする

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer/vd1/ /Dest:C:\myfolder /SourceKey:key /S

以下の BLOB が指定されたコンテナーにあるとします。

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

コピー操作後にディレクトリ `C:\myfolder` に含まれるのは以下のファイルです。 仮想ディレクトリ内の BLOB のみがコピーされることに注意してください。

    C:\myfolder\a.txt
    C:\myfolder\abcd.txt

### 指定されたファイル パターンに一致するファイルをコンテナーに再帰的にアップロードする 

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

フォルダー `C:\myfolder` に以下のファイルがあるとします。

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

コピー操作後にコンテナーに含まれるのは以下のファイルです。

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt
    
### 指定されたプレフィックスを付けた BLOB をファイル システムに再帰的にダウンロードする

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

以下の BLOB が指定されたコンテナーにあるとします。 プレフィックス `a` で始まるすべての BLOB がコピーされます。

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

コピー操作後にフォルダー `C:\myfolder` に含まれるのは以下のファイルです。

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

プレフィックスは仮想ディレクトリに適用され、BLOB 名の最初の部分を構成することに注意してください。 前の例では、仮想ディレクトリは指定されたプレフィックスに一致しないため、コピーされません。


### BLOB とそのスナップショットの別のストレージ アカウントへのコピー

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

コピー操作後、ターゲット コンテナーには BLOB とそのスナップショットが含まれます。 前の例の BLOB に 2 つのスナップショットが含まれる場合、コンテナーには以下の BLOB とスナップショットが含まれます。

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt


### コマンド ライン パラメーターを指定するための応答ファイルの使用

    AzCopy /@:"C:\myfolder\abc.txt"

応答ファイルには、任意の AzCopy コマンド ライン パラメーターを含めることができます。 AzCopy は、ファイルの内容に直接置換することで、ファイルのパラメーターをコマンド ラインで指定されているかのように処理します。

**1 つ以上の単一行の応答ファイルを指定する**

`source.txt` という名前の応答ファイルが、ソース コンテナーを次のように指定するとします。

    /Source:http://myaccount.blob.core.windows.net/mycontainer

さらに、`dest.txt` という名前の応答ファイルは、ファイル システムの宛先フォルダーを次のように指定します。

    /Dest:C:\myfolder

また、`options.txt` という名前の応答ファイルは、AzCopy のオプションを次のように指定します。

    /S /Y

すべてディレクトリ `C:\responsefiles` に格納されているこれらの応答ファイルを使用して AzCopy を呼び出すには、次のコマンドを使用します。

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy は次のように、個々のパラメーターがすべてコマンド ラインで指定されたかのようにこのコマンドを処理します。

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

**複数行の応答ファイルを指定する**

`copyoperation.txt` という名前のファイルに、以下の行が含まれているとします。 AzCopy の各パラメーターは次のように、それぞれ個別の行に指定されます。

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S 
    /Y

この応答ファイルを使用して AzCopy を呼び出すには、次のコマンドを使用します。

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

AzCopy は次のように、個々のパラメーターがすべてコマンド ラインで指定されたかのようにこのコマンドを処理します。    

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

各 AzCopy パラメーターはそれぞれ 1 行で指定する必要があります。 たとえば、次の `/sourcekey` パラメーターで示すように、パラメーターが 2 行に分かれていると、AzCopy は失敗します。

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    [sourcekey]
    /S 
    /Y

### 共有アクセス署名 (SAS) の指定
    
**/sourceSAS オプションを使用してソース コンテナーに SAS を指定する**

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /DestC:\myfolder /SourceSAS:SAS /S

**ソース コンテナーの URI でソース コンテナーに SAS を指定する**

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

**/destSAS オプションを使用して宛先コンテナーに SAS を指定する**

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer1 /DestSAS:SAS /Pattern:abc.txt

**ソースと宛先のコンテナーに SAS を指定する**

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

### ジャーナル ファイル フォルダーの指定

AzCopy にコマンドが発行されるたびに、AzCopy は既定のフォルダーまたはこのオプションで指定されたフォルダーにジャーナル ファイルが存在するかどうかを確認します。 どちらの場所にもジャーナル ファイルがない場合は、AzCopy は新しい操作であると認識し、新しいジャーナル ファイルを作成します。

既存のジャーナル ファイルがある場合は、AzCopy は入力されたコマンド ラインがジャーナル ファイルのコマンド ラインと一致するかどうかを確認します。 2 つのコマンド ラインが一致する場合、AzCopy は未完了の操作を再開します。 一致しない場合は、ジャーナル ファイルを上書きして新しい操作を開始するか、現在の操作を取り消すかをたずねるメッセージが表示されます。 

**既定の場所をジャーナル ファイルに使用する**

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

前に示したように、オプション `/Z` を省略するか、フォルダー パスなしでオプション `/Z` を指定すると、AzCopy は既定の場所である `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy` にジャーナル ファイルを作成します。 既存のジャーナル ファイルがある場合、このジャーナル ファイルに基づいて AzCopy が操作を再開します。 

**ジャーナル ファイルにカスタムの場所を指定する**

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

この例では、既存のジャーナル ファイルがない場合に、これを作成します。 既存のファイルがある場合、AzCopy はこのジャーナル ファイルに基づいて操作を再開します。

**AzCopy の操作を再開する**

    AzCopy /Z:C:\journalfolder\

この例では、完了できなかった可能性がある直前の操作を再開します。


### ログ ファイルの生成

**詳細ログ ファイルをデフォルトの場所に書き込む**

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

冗長ログへのファイル パスを指定せずにオプション `/V` を指定すると、AzCopy は既定の場所である `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy` にログ ファイルを作成します。

**詳細ログ ファイルをカスタムの場所に書き込む**

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

`/V:test/azcopy1.log` のように、オプション `/V` に続けて相対パスを指定すると、現在の作業ディレクトリの `test` という名前のサブフォルダー内に詳細ログが作成されます。


### ダウンロードしたファイルの最終変更時刻をソース BLOB と同時刻に設定する

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

### 最終変更時刻に基づいてコピー操作の対象から BLOB を除外する

`/MT` オプションを指定して、ソース BLOB と宛先ファイルの最終変更時刻を比較します。

**最終変更時刻が宛先ファイルと同じかそれよりも後の BLOB を除外**

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

**最終変更時刻が宛先ファイルと同じかそれよりも前の BLOB を除外**

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

### 開始する同時操作の数の指定

オプション `/NC` を使用して同時コピー操作の数を指定します。 既定では、AzCopy は、環境で使用されているコア プロセッサ数の 8 倍の数の同時操作を開始します。 低帯域幅のネットワークで AzCopy を実行している場合は、このオプションに少ない数を指定することで、リソースの競合で生じる失敗を回避できます。


###     ストレージ エミュレーターの BLOB リソースに対する AzCopy の実行

    AzCopy /Source:https://127.0.0.1:10004/myaccount/myfileshare/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

### 2 つの Azure ストレージ エンドポイント間で BLOB を同期的にコピーする

既定では、AzCopy は 2 つのストレージ エンドポイント間で非同期でデータをコピーします。 そのため、コピー操作は、BLOB のコピー速度に関する SLA のない予備の帯域幅容量を使用してバック グラウンドで実行され、AzCopy はコピーが完了または失敗するまで定期的にコピー状態を確認します。 

`/SyncCopy` オプションは、コピー操作が一定の速度になるようにします。 AzCopy は、指定されたソースからローカル メモリにコピーして BLOB をダウンロードし、これを BLOB ストレージのコピー先にアップロードすることで同期コピーを実行します。

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy` は、非同期コピーと比較すると追加の送信コストが発生する可能性があります。送信コストを回避するために、ソース ストレージ アカウントと同じリージョンにある Azure VM でこのオプションを使用することをお勧めします。

### コピー先の BLOB の MIME コンテンツの種類を指定する

既定では、AzCopy はコピー先の BLOB のコンテンツの種類を `application/octet-stream` に設定します。 バージョン 3.1.0 以降は、オプション `/SetContentType:[content-type]` を使用してコンテンツの種類を明示的に指定できます。 この構文は、コピー操作におけるすべての BLOB のコンテンツの種類を設定します。

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

`/SetContentType` で値を指定しない場合、AzCopy は各 BLOB またはファイルのコンテンツの種類をそのファイルの拡張子に応じて設定します。

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## AzCopy を使用した Azure ファイル ストレージでのファイル コピー

以下の例は、AzCopy を使用して Azure ファイルをコピーするさまざまなシナリオを示したものです。

### Azure ファイル共有のファイルをファイル システムにダウンロードする

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

指定されたソースが Azure ファイル共有の場合、必要がありますか、指定厳密なファイル名 (*など* `abc.txt`) 1 つのファイルをコピーまたはオプションを指定する `/S` 共有再帰的にすべてのファイルをコピーします。 ファイル パターンとオプション `/S` の両方を同時に指定しようとすると、エラーになります。

### Azure ファイル共有のファイルおよびフォルダーをファイル システムにダウンロードし、再帰的に共有アクセス署名を指定する

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceSAS:SAS /S

空のフォルダーはコピーされないことに注意してください。


### ファイル システムのファイルおよびフォルダーを Azure ファイル共有に再帰的にアップロードする

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

空のフォルダーはコピーされないことに注意してください。


### 指定のファイル パターンに一致するファイルを Azure ファイル共有に再帰的にアップロードする

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

### Azure File ストレージでファイルを非同期的にコピーする

Azure File ストレージでは、サーバー側の非同期コピーがサポートされています。

File Storage 間の非同期コピー:

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

File Storage からブロック BLOB への非同期コピー
  
    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

ブロック/ページ Blob Storage から File Storage への非同期コピー:

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

File Storage からページ BLOB への非同期コピーはサポートされていません。

### File Storage でファイルを同期的にコピーする

非同期コピーの他に、オプション `/SyncCopy` を指定して File Storage から File Storage に、File Storage から Blog Storage に、および Blog Storage から File Storage にデータを同期コピーすることもできます。AzCopy はソース データをローカル メモリーにダウンロードして、それを宛先に再度アップロードすることでこれを行います。

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy
    
    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

File Storage から Blob Storage にコピーするときの既定の BLOB タイプはブロック BLOB ですが、ユーザーはオプション `/BlobType:page` を指定して宛先 BLOB タイプを変更できます。

`/SyncCopy` は、非同期コピーと比較すると追加の送信コストが発生する可能性があります。送信コストを回避するために、ソース ストレージ アカウントと同じリージョンにある Azure VM でこのオプションを使用することをお勧めします。


## AzCopy を使用した Azure テーブルでのエンティティのコピー

以下の例は、AzCopy を使用して Azure テーブル エンティティをコピーするさまざまなシナリオを示したものです。

### エンティティをローカル ファイル システムにエクスポートする

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy により、指定した宛先フォルダーまたは BLOB コンテナーにマニフェスト ファイルが作成されます。 このマニフェスト ファイルは、インポート処理中に、必要なデータ ファイルを特定し、データの検証を実行するために使用されます。 マニフェスト ファイルでは、既定で次の名前付け規則が使用されます。

    <account name>_<table name>_<timestamp>.manifest

オプション `/Manifest:<manifest file name>` を指定してマニフェスト ファイル名を設定することもできます。

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest


### JSON および CSV データ ファイル形式へのエンティティのエクスポート

既定では、AzCopy はテーブル エンティティを JSON ファイルにエクスポートしますが、ユーザーはオプション `/PayloadFormat:JSON|CSV` を指定してエクスポートされるデータ ファイル タイプを決定できます。

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

CSV ペイロード形式を指定すると、AzCopy は、パラメーター `/Dest` で指定した場所にある `.csv` 拡張子を持つデータ ファイルの他に、データ ファイルごとにファイル拡張子 `.schema.csv` を持つスキーマ ファイルを生成します。
AzCopy には CSV データ ファイルの「インポート」のサポートは含まれていませんが、JSON 形式を使用してテーブルのエクスポートおよびインポートを行うことができます。

### エンティティを Azure BLOB にエクスポートする

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy は、次の名前付け規則を使用して JSON データ ファイルをローカル フォルダーまたは BLOB コンテナーに生成します。

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

生成される JSON データ ファイルは、最小のメタデータ用のペイロード形式に準じます。 このペイロード形式の詳細については、「 [テーブル サービス操作のペイロード形式](http://msdn.microsoft.com/library/azure/dn535600.aspx)します。

注意してくださいストレージ Blob をストレージ テーブル エンティティをエクスポートするときに AzCopy はまずテーブル エンティティをローカルの一時データ ファイルにエクスポートされ、Blob にアップロードし、これらの一時データ ファイルは既定のパスにジャーナル ファイル フォルダーに格納"<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>"、/z: [ジャーナル ファイル フォルダー] ジャーナル ファイル フォルダーの場所を変更し、一時データ ファイルの場所を変更するためのオプションを指定することができます。 一時データ ファイルのサイズは、テーブル エンティティのサイズと、オプション /SplitSize で指定したサイズによって決まります。ローカル ディスクの一時データ ファイルは BLOB にアップロードされた後すぐに削除されますが、そのような一時データ ファイルを格納するのに十分なローカル ディスク領域が存在することを事前に確認しておく必要があります。 

### エクスポート ファイルを分割する

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy を使用して、 *ボリューム インデックス* で複数のファイルを区別するために、分割したデータ ファイル名。 ボリューム インデックスは、2 つの部分で構成される、 *パーティション キー範囲のインデックス* と *分割ファイルのインデックス*します。 どちらのインデックスも 0 から始まります。

パーティション キー範囲のインデックスは、ユーザーがオプション `/PKRS` を指定しなかった場合、0 に設定されます (次のセクションで説明します)。

たとえば、ユーザーがオプション `/SplitSize` を指定した後に、AzCopy によって 2 つのデータ ファイルが生成されたとします。 生成されたデータ ファイルの名前は次のようになります。

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

オプション `/SplitSize` の最小の値は 32 MB です。 指定された宛先が BLOB ストレージの場合、データ ファイルのサイズが BLOB サイズの上限 (200 GB) に達すると、ユーザーがオプション `/SplitSize` を指定しているかどうかにかかわらず、AzCopy によってデータ ファイルが分割されます。

### エンティティを並列処理でエクスポートする

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

ユーザーによってオプション `/PKRS` が指定されると、AzCopy はエンティティをエクスポートするための同時操作を開始します。 1 つの操作で 1 つのパーティション キー範囲がエクスポートされます。

同時操作の数もオプション `/NC` によって制御されます。 AzCopy はコア プロセッサの数を使用しての既定値として `/NC` テーブル エンティティをコピーするときにも `/NC` が指定されていません。 ユーザーがオプション `/PKRS` を指定した場合、AzCopy は、パーティション キー範囲と明示的または暗黙的に指定された同時操作数の 2 つの値のうち小さい方を使用して、開始する同時操作の数を決定します。 詳細については、コマンド ラインに「`AzCopy /?:NC`」と入力してください。

### エンティティを並列処理でインポートする

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace 

オプション `/EntityOperation` は、エンティティをテーブルに挿入する方法を示します。 次のいずれかの値になります。

- `InsertOrSkip`: テーブルにエンティティが存在する場合はスキップし、存在しない場合は新しいエンティティを挿入します。
- `InsertOrMerge`: テーブルにエンティティが存在する場合はマージし、存在しない場合は新しいエンティティを挿入します。
- `InsertOrReplace`: テーブルにエンティティが存在する場合は置換し、存在しない場合は新しいエンティティを挿入します。

インポート シナリオでは、オプション `/PKRS` を指定できません。 同時操作を開始するためにオプション `/PKRS` を指定する必要があるエクスポート シナリオとは違って、エンティティをインポートすると、既定で同時操作が開始されます。 開始される同時操作の既定数は、コア プロセッサの数と同じです。ただし、オプション `/NC` を使用して別の同時操作数を指定できます。 詳細については、コマンド ラインに「`AzCopy /?:NC`」と入力してください。


## 既知の問題とベスト プラクティス

#### 1 つのマシンでは 1 つの AzCopy インスタンスを実行します。
AzCopy はマシン リソースの利用率を最大限に高めてデータ転送を高速化する設計になっています。1 つのマシンで実行する AzCopy インスタンスは 1 つのみとすること、同時実行操作の数を増やす必要がある場合はオプション `/NC` を指定することをお勧めします。 詳細については、コマンド ラインに「`AzCopy /?:NC`」と入力してください。

#### [暗号化、ハッシュ、署名のための FIPS 準拠アルゴリズムを使う] を有効にする場合、AzCopy に対して FIPS 準拠の MD5 アルゴリズムを有効にします。
AzCopy はオブジェクトをコピーするときに、既定で .NET MD5 実装を使用して MD5 を計算しますが、AzCopy で FIPS 準拠の MD5 設定を有効にするために必要ないくつかのセキュリティ要件があります。

プロパティ `AzureStorageUseV1MD5` を含む App.config ファイル `AzCopy.exe.config` を作成し、それを AzCopy.exe と共に取っておくことができます。

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

"AzureStorageUseV1MD5"プロパティの
• True - 既定値では、AzCopy は .NET MD5 実装を使用します。
• False – AzCopy は FIPS 準拠の MD5 アルゴリズムを使用します。

FIPS 準拠アルゴリズムは既定で Windows マシンでは無効に設定されます。secpol.msc を Run ウィンドウに入力し、この切り替えを [セキュリティ設定]、[ローカル ポリシー]、[セキュリティのオプション]、[システム暗号化] の順にクリックして確認できます。暗号化、ハッシュ、署名には、FIPS 準拠のアルゴリズムを使用します。

## AzCopy のバージョン

> [AZURE.NOTE] 私たちは、最新バージョンの新機能とパフォーマンスを向上させるための AzCopy をインストールするコンパイルします。

| バージョン | 新機能                                                                                                      | 参照される .NET クライアント ライブラリのバージョン | ターゲット ストレージの REST API バージョン |
|---------|-----------------------------------------------------------------------------------------------------------------|--------|----------|
| [**V5.0.0**](http://xdmrelease.blob.core.windows.net/azcopy-5-0-0/MicrosoftAzureStorageTools.msi)  | **最新リリース バージョン。 V4.2.0 のすべての機能が含まれています。 ここではファイルおよびテーブル ストレージの機能も公開状** | **V6.0.0** | **2015-04-05**
| [V4.2.0](http://xdmrelease.blob.core.windows.net/azcopy-4-2-0-preview/MicrosoftAzureStorageTools.msi)  | V3.2.0 のすべての機能が含まれています。 また、File Storage 共有 SAS、File Storage 非同期コピー、CSV へのテーブル エンティティのエクスポート、テーブル エンティティをエクスポートするときのマニフェスト名の指定もサポートされます。 | V5.0.0 | 2015-02-21
| [V3.2.0](http://xdmrelease.blob.core.windows.net/azcopy-3-2-0/MicrosoftAzureStorageTools.msi)  | 追加 BLOB と FIPS 準拠 MD5 設定をサポートします。 | V5.0.0 | 2015-02-21
| [V4.1.0](http://xdmrelease.blob.core.windows.net/azcopy-4-1-0-preview/MicrosoftAzureStorageTools.msi)  | V3.1.0 のすべての機能が含まれています。 BLOB およびファイルの同期コピーと、コピー先の BLOB またはファイルへのコンテンツの種類の指定をサポートします。 | V4.3.0 | 2014-02-14
| [V3.1.0](http://xdmrelease.blob.core.windows.net/azcopy-3-1-0/MicrosoftAzureStorageTools.msi)  | BLOB の同期コピーと、コピー先の BLOB へのコンテンツの種類の指定をサポートします。| V4.3.0 | 2014-02-14
| [V4.0.0](http://xdmrelease.blob.core.windows.net/azcopy-4-0-0-preview/MicrosoftAzureStorageTools.msi)  | V3.0.0 のすべての機能が含まれています。 Azure ファイル ストレージとの間のファイルのコピーと Azure テーブル ストレージとの間のエンティティのコピーもサポートしています。| V4.2.1 | 2014-02-14
| [V3.0.0](http://xdmrelease.blob.core.windows.net/azcopy-3-0-0/MicrosoftAzureStorageTools.msi)  | AzCopy コマンド ライン構文が変更され、パラメーター名が必要になりました。また、コマンド ライン ヘルプが再設計されています。 このバージョンでは、Azure BLOB ストレージとの間のコピーのみをサポートしています。| V4.2.1 | 2014-02-14
| V2.5.1  | オプション /xo と /xn を使用したときのパフォーマンスが最適化されました。 ソース ファイル名に含まれた特殊文字に関連するバグと、ユーザーが誤ったコマンド ライン構文を入力した後のジャーナル ファイルの損傷が解決されました。| V4.1.0 | 2014-02-14
| V2.5.0  | 大規模なコピーのシナリオでのパフォーマンスが最大化され、使いやすさの点でいくつかの重要な改善が加えられました。| V4.1.0 | 2014-02-14
| V2.4.1  | インストール ウィザードでの宛先フォルダーの指定をサポートします。| V4.0.0 | 2014-02-14
| V2.4.0  | Azure ファイル ストレージに対するファイルのアップロードとダウンロードをサポートします。| V4.0.0 | 2014-02-14
| V2.3.0  | Geo 冗長ストレージ アカウントの読み取りアクセスをサポートします。| V3.0.3 | 2013-08-15
| V2.2.2  | Azure ストレージ クライアント ライブラリのバージョン 3.0.3 を使用できるようにアップグレードされました。| V3.0.3 | 2013-08-15
| V2.2.1  | 同一ストレージ アカウント内での大量のファイル コピーで発生していたパフォーマンスの問題が解決されました。| V2.1.0 |
| V2.2    | BLOB 名に対して仮想ディレクトリの区切り記号を設定する操作がサポートされています。 ジャーナル ファイルのパスの指定がサポートされています。| V2.1.0 |
| V2.1    | 効果的な BLOB のアップロード、ダウンロード、およびコピーのために 20 個を超えるオプションが提供されています。| V2.0.5 |


## 次のステップ

Azure Storage および AzCopy の詳細については、以下のリソースを参照してください。

### Azure Storage のドキュメント:

- [Microsoft Azure Storage の概要](storage-introduction.md)
- [.NET から Blob Storage を使用する方法](storage-dotnet-how-to-use-blobs.md)
- [Java からファイル ストレージを使用する方法](storage-dotnet-how-to-use-files.md)
- [.NET からテーブル ストレージを使用する方法](storage-dotnet-how-to-use-tables.md)
- [ストレージ アカウントを作成、管理、または削除する方法](storage-create-storage-account.md)
- [Import/Export Service を使用した Blob Storage へのデータの転送](storage-import-export-service.md)

### Azure Storage に関するブログの投稿:
- [DML: azure ストレージの概要のデータの移動ライブラリ プレビュー](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: 同期コピーとカスタマイズしたコンテンツの種類の概要](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Announcing General Availability of AzCopy 3.0 plus preview release of AzCopy 4.0 with Table and File support (AzCopy: AzCopy 3.0 の一般公開とテーブルおよびファイルのサポートを伴う AzCopy 4.0 のプレビュー リリースのお知らせ)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Optimized for Large-Scale Copy Scenarios (AzCopy: 大量のコピーのシナリオ用の最適化)](http://go.microsoft.com/fwlink/?LinkId=507682)
- [Introducing Microsoft Azure File Service (Microsoft Azure File サービスの概要)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
- [AzCopy - Support for read-access geo-redundant storage (AzCopy - Geo 冗長ストレージの読み取りアクセスのサポート)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy - Transfer data with re-startable mode and SAS token (AzCopy - 再起動可能モードまたは SAS トークンを使用したデータの転送)](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy - Using cross-account Copy Blob (AzCopy - アカウント間での BLOB のコピー)](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy - Uploading/downloading files for Azure Blobs (AzCopy - Azure BLOB に対するファイルのアップロードおよびダウンロード)](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)

