<properties
   pageTitle="Reliable Service のバックアップと復元 | Microsoft Azure"
   description="Service Fabric Reliable Service のバックアップと復元の概念をまとめた文書"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="subramar,jessebenson"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/01/2015"
   ms.author="mcoskun"/>

# Reliable Services のバックアップと復元

Service Fabric は高可用性プラットフォームであり、複数のノードにわたりステートを複製し、その高可用性を維持します。  つまり、クラスター内の 1 つのノードに障害が発生した場合でも、サービスは引き続き利用できます。 このプラットフォームに組み込まれている冗長性で十分な場合もありますが、(外部ストアに) サービスのデータをバックアップすることが望ましくなります。

たとえば、次のシナリオではサービスのデータをバックアップすることが望まれます。

* Service Fabric クラスター全体または特定のパーティションを実行しているすべてのノードが永久に失われた場合。 これは、たとえば、geo レプリケーションされないとき、クラスター全体が 1 か所のデータ センターにあり、データ センター全体が停止する場合などに起こります。

* ステートが誤って削除される/壊れるなどの管理エラー。 これは、十分な権限を持つ管理者が誤ってサービスを削除した場合に起こる可能性があります。

* サービスのバグが引き起こしたデータの破損。 これは、サービス コードのアップグレードが、Reliable Collection に不正なデータを書き込み始めると起こる可能性があります。 このような場合は、コードとデータの両方を、以前の状態に復元する必要があります。

* オフライン データ処理 データを生成するサービスから離れているビジネス インテリジェンスのためにデータのオフライン処理を用意しておけば便利です。

バックアップ/復元機能では、 Reliable Services API を基盤とするサービスでバックアップを作成し、復元できます。 このプラットフォームが提供するバックアップ API では、読み書き操作をブロックせずにパーティションのステートをバックアップできます。 復元 API では、選択したバックアップからパーティションのステートを復元できます。


## バックアップの方法

サービスの作成者は、バックアップのタイミングと保管場所を完全に制御できます。

サービスをバックアップを開始するを呼び出す必要があります **IReliableStateManager.BackupAsync**します。  バックアップはプライマリ レプリカ以外からは実行できません。書き込みステータスを与える必要があります。

最も単純なオーバー ロードは、次のように **BackupAsync** Func << BackupInfo、bool >> と呼ばれるは、 **backupCallback**します。

```C#
await this.StateManager.BackupAsync(this.BackupCallbackAsync);
```

**BackupInfo** 、ランタイムが (BackupInfo.Directory) のバックアップを保存するフォルダーの場所を含むバックアップに関する情報を提供します。 このコールバック関数は BackupInfo.Directory が外部ストアまたは別の場所に移動することを要求します。  この関数はまた、バックアップ フォルダーがそのターゲット フォルダーに移動したかどうかを示すブール値を返します。

次のコードは、backupCallback を利用し、バックアップを Azure Storage にアップロードするしくみを示しています。

```C#
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, CancellationToken.None);

    return true;
}
```

上記の例では、 **ExternalBackupStore** ために使用されるサンプル クラスは、Azure Blob ストレージと連携して **UploadBackupFolderAsync** は、メソッドは、フォルダーを圧縮し、それを Azure Blob ストアに配置します。

注意:

- ある 1 つになります **BackupAsync** いつでもレプリカ インフライトごとの特定の時点を指定します。 1 つ以上 **BackupAsync** 回の呼び出しではスロー **FabricBackupInProgressException** いずれかに転送中のバックアップを制限します。

- バックアップの途中でレプリカにエラーが発生した場合、バックアップは完了しないことがあります。 したがって、フェイル オーバーが完了するが呼び出すことによって、バックアップを再開するサービスの責任 **BackupAsync** に応じて。

## データの復元方法

通常、復元操作が必要になるケースは、次のカテゴリのいずれかに分類されます。


1. サービス パーティションのデータが失われた場合。 たとえば、パーティションの 3 つのレプリカのうちの 2 つのディスク (プライマリ レプリカを含む) が壊れた/消去された場合です。 新しいプライマリに、バックアップからデータを復元する必要がある場合。

2. サービス全体が失われた場合。 たとえば、管理者がサービス全体を削除し、サービスとデータを復元する必要がある場合です。

3. (たとえば、アプリケーション バグに起因して) サービスが壊れたアプリケーション データを複製した場合。 その場合、サービスをアップグレードするか、元の状態に戻し、データが壊れるの原因を取り除き、壊れていないデータを復元する必要があります。

さまざまな方法が可能ですが、RestoreAsync で上記のシナリオから復元する方法について紹介します。

## パーティション データの損失

この場合、ランタイムは自動的に検出データの損失して呼び出し、 **OnDataLossAsync** API です。

サービス作成者は、次を実行して回復する必要があります。
- オーバーライド **IReliableStateManager** を新しいのする ReliableStateManager を返して、データ損失の場合に呼び出されるコールバック関数の呼び出しを提供します。
- サービスのバックアップを格納する外部の場所で最新のバックアップを見つけます。
- 最新のバックアップの状態が新しいプライマリの背後にある場合は、false を返します。 これにより、新しいプライマリが書き込まれないこと取得過剰古いデータが保証されます。
- 最新のバックアップをダウンロードします (およびそれが圧縮されている場合は、バックアップ フォルダーにバックアップを解凍します)。
- 呼び出す **IReliableStateManager.RestoreAsync** をバックアップ フォルダーへのパス。
- 復元が成功した場合は、true を返します。

実装例を次に示します **OnDataLossAsync** メソッドと共に、 **IReliableStateManager** をオーバーライドします。

```C#
protected override IReliableStateManager CreateReliableStateManager()
{
    return new ReliableStateManager(new ReliableStateManagerConfiguration(
            onDataLossEvent: this.OnDataLossAsync));
}

protected override async Task<bool> OnDataLossAsync(CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    await this.StateManager.RestoreAsync(backupFolder);

    return true;
}
```

>[AZURE.NOTE] 既定では、RestorePolicy は Safe に設定します。  つまり、バックアップ フォルダーにこのレプリカに含まれているステートと同じか、それより古いステートが含まれていることが検出された場合、RestoreAsync API は失敗し、ArgumentException をスローします。  RestorePolicy.Force でこの安全性チェックを省略できます。

## サービスの削除または紛失

サービスが削除された場合、先にサービスを作成し直さないとデータを復元できません。  データを途切れなく復元するために、サービスを同じ構成 (パーティショニング スキームなど) で作成することが重要です。  サービスは、データを復元する API を (**OnDataLossAsync** 上) にこのサービスのすべてのパーティションで呼び出すことができます。  使用これを実現するための方法の 1 つ **FabricClient.ServiceManager.InvokeDataLossAsync** のすべてのパーティションにします。  

この時点から、実装は上記のシナリオと同じになります。  各パーティションで、外部ストアから最新の関連バックアップを復元する必要があります。 1 つ注意してことがあります。ランタイムが (動的に) パーティション ID を作成するため、パーティション ID が変更されている可能性があります。 そのため、各パーティションで復元する最新バックアップを識別するために、サービスでパーティション情報とサービス名を格納しておく必要があります。


## 壊れたアプリケーション データの複製

新しくデプロイしたアプリケーションのアップグレードにバグがある場合、データが壊れてしまうことがあります。たとえば、アプリケーション アップグレードによって、Reliable Dictionary にあるすべての電話番号の記録が無効な局番で更新されてしまいます。  そのような場合、Service Fabric は保存されるデータの性質を認識しないため、無効な電話番号が複製されます。

データを壊すこのような重大なバグが検出された場合、最初にするべきことは、サービスをアプリケーション レベルで停止し、可能であれば、バグのないバージョンのアプリケーション コードにアップグレードします。  しかしながら、サービス コードを修正しても、データは壊れたままであり、復元する必要があります。  そのような場合、最新バックアップも壊れている可能性があるので、最新バックアップを復元するだけでは不十分です。  そのため、データが壊れる前に行われた最後のバックアップを探す必要があります。

どのバックアップが破損しているかがわからない場合は、新しい Service Fabric クラスターをデプロイし、上記の "サービスの削除" のシナリオと同様に、影響を受けたパーティションのバックアップを復元できます。  各パーティションで、最新から一番古いものの順でバックアップの復元を開始します。 破損していないバックアップを見つけたら、このパーティションの (そのバックアップより) 新しいすべてのバックアップを移動、削除します。 各パーティションでこの手順を繰り返します。 今すぐときに、 **OnDataLossAsync** と呼ばれる運用環境のクラスター内のパーティションで最後のバックアップを外部ストアにあるものになります、上記の手順で取得します。

「サービスの削除」の手順を利用し、サービス バックアップのステートをバグのあるコードがステートを壊す前のステートに戻すことができます。

以下の点に注意してください。

- 復元の際、復元されるバックアップがデータ損失前のパーティションの状態よりも古くなっている可能性があります。 そのため、復元は、可能な限り多くのデータを復元するための最後の手段として使用するべきです。

- バックアップ フォルダー パスとバックアップ フォルダー内のファイルのパスを表す文字列は、FabricDataRoot パスや Application Type 名の長さによっては 255 文字を超える可能性があります。 一部の .Net メソッドのような可能性があります **Directory.Move** をスローする、 **PathTooLongException**します。 1 つの回避策はのような kernel32 Api を直接呼び出すを **CopyFile**します。


## 内部: バックアップと復元に関する追加情報

### Backup
Reliable State Manager には、読み書き操作を中断することなく、一貫性のあるバックアップを作成する機能があります。 そのために、チェックポイントとログ永続化のメカニズムが活用されます。  Reliable State Manager は特定の時点でファジー (簡易) チェックポイントを作成し、トランザクション ログからの負荷を軽減し、復元時間を早めます。  ときに IReliableStateManager**。BackupAsync** が呼び出され、Reliable State Manager 信頼性の高いすべてのオブジェクトに指示を最新のチェックポイント ファイルをローカルのバックアップ フォルダーにコピーします。  次に、Reliable State Manager は「開始ポインター」から最新のログ レコードまですべてのログ レコードをバックアップ フォルダーにコピーします。  ログ レコードが、最新のログ レコードまで、すべてバックアップに追加され、Reliable State Manager が Write Ahead Logging を維持するため、Reliable State Manager では、コミットされた (CommitAsync が正常に返ります) すべてのトランザクションがバックアップに含まれることが保証されます。

後にコミットするすべてのトランザクション、 **BackupAsync** が呼び出されて、ことがあります、またはバックアップではありません。  プラットフォームによりローカルのバックアップ フォルダーにデータが入力されると (すなわち、ローカルのバックアップがランタイムにより完了すると)、サービスのバックアップ コールバックが呼び出されます。  このコールバックは、Azure Storage などの外部の場所にバックアップ フォルダーを移動する役割を担います。

### 復元

Reliable State Manager には、IReliableStateManager.RestoreAsync API でバックアップから復元する機能があります。  RestoreAsync メソッドは、内部のみ呼び出すことができる、 **OnDataLossAsync** メソッドです。  によって返される bool **OnDataLossAsync** 、サービスが、外部ソースからの状態を復元するかどうかを示します。  場合、 **OnDataLossAsync** Service Fabric には、このプライマリからの他のすべてのレプリカが再構築に true を返します。  Service Fabric によりが受信するレプリカ **OnDataLossAsync** プライマリ ロールに移行先が、読み取りの状態または状態の書き込みは与えられません。  つまり、ある StatefulService 実装についての RunAsync が呼び出されないまで **OnDataLossAsync** が正常に完了します。  次に、 **OnDataLossAsync** は新しいプライマリに対して呼び出されます。 サービスが (true または false を返し) この API を完了し、関連再構成を完了するまで、API は 1 つずつ呼び出され続けます。


RestoreAsync は最初、それが呼び出されたプライマリ レプリカの既存ステートをすべて削除します。  次に、Reliable State Manager はバックアップ フォルダーに存在するすべての Reliable Objects を作成します。  次に、Reliable Objects はバックアップ フォルダーのチェックポイントから復元するように指示されます。  最後に、Reliable State Manager はバックアップ フォルダーのログ レコードからそれ自体のステートを復元し、復元を実行します。  復元プロセスの一環として、バックアップ フォルダーにコミット ログ レコードがあり、「開始ポイント」から始まる操作が Reliable Objects でリプレイされます。  この手順により、復元したステートに一貫性が与えられます。

