---
title: 'Azure PowerShell: SQL データベースを作成する | Microsoft Docs'
description: Azure Portal で SQL Database の論理サーバー、サーバーレベルのファイアウォール規則、データベースを作成する方法について説明します。
keywords: SQL データベース チュートリアル, SQL データベースの作成
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.devlang: PowerShell
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 205e2c075f98ebb6fe7bd6156fd255d25c434681
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-single-azure-sql-database-using-powershell"></a>PowerShell を使用して単一の Azure SQL データベースを作成する

PowerShell は、コマンドラインやスクリプトで Azure リソースを作成および管理するために使用します。 このガイドでは、PowerShell を使用して Azure SQL データベースを [Azure SQL Database 論理サーバー](sql-database-features.md)内の [Azure リソース グループ](../azure-resource-manager/resource-group-overview.md)にデプロイする方法について詳しく説明します。

Azure サブスクリプションをお持ちでない場合は、開始する前に[無料](https://azure.microsoft.com/free/)アカウントを作成してください。

このチュートリアルには、Azure PowerShell モジュール バージョン 4.0 以降が必要です。 バージョンを確認するには、` Get-Module -ListAvailable AzureRM` を実行します。 インストールまたはアップグレードする必要がある場合は、[Azure PowerShell モジュールのインストール](/powershell/azure/install-azurerm-ps)に関するページを参照してください。

## <a name="log-in-to-azure"></a>Azure にログインする

[Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) コマンドで Azure サブスクリプションにログインし、画面上の指示に従います。

```powershell
Connect-AzureRmAccount
```

## <a name="create-variables"></a>変数の作成

このクイック スタートのスクリプトで使用する変数を定義します。

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a>リソース グループの作成

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) コマンドで [Azure リソース グループ](../azure-resource-manager/resource-group-overview.md)を作成します。 リソース グループとは、複数の Azure リソースをまとめてデプロイ、管理する際の論理コンテナーです。 次の例では、`myResourceGroup` という名前のリソース グループを `westeurope` の場所に作成します。

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>論理サーバーの作成

[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) コマンドで [Azure SQL Database 論理サーバー](sql-database-features.md)を作成します。 論理サーバーには、ひとまとめにして管理される一連のデータベースが含まれています。 次の例では、管理者ログイン `ServerAdmin` とパスワード `ChangeYourAdminPassword1` を使用し、ランダムに名前を付けたサーバーをリソース グループ内に作成しています。 これらの定義済みの値は、必要に応じて別の値に置き換えてください。

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>サーバーのファイアウォール規則の構成

[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) コマンドで [Azure SQL Database のサーバーレベルのファイアウォール規則](sql-database-firewall-configure.md)を作成します。 サーバーレベルのファイアウォール規則を作成すると、SQL Server Management Studio や SQLCMD ユーティリティのような外部アプリケーションから、SQL Database サービスのファイアウォールを介して SQL データベースに接続できます。 次の例では、他の Azure リソースに対してのみファイアウォールを開放しています。 外部から接続できるようにするには、実際の環境に合わせて IP アドレスを変更してください。 すべての IP アドレスを開放するには、開始 IP アドレスとして 0.0.0.0 を、終了アドレスとして 255.255.255.255 を使用します。

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL Database の通信は、ポート 1433 上で行われます。 企業ネットワーク内から接続しようとしても、ポート 1433 での送信トラフィックがネットワークのファイアウォールで禁止されている場合があります。 その場合、会社の IT 部門によってポート 1433 が開放されない限り、Azure SQL Database サーバーに接続することはできません。
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>サンプル データを使用したサーバーのデータベースの作成

[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) コマンドでサーバー内に [S0 パフォーマンス レベル](sql-database-service-tiers.md)のデータベースを作成します。 次の例では、`mySampleDatabase` というデータベースを作成し、このデータベースに AdventureWorksLT のサンプル データを読み込みます。 必要に応じて、これらの定義済みの値を置き換えてください (このコレクションの他のクイック スタート チュートリアルは、このクイック スタートの値に基づいています)。

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>リソースのクリーンアップ

このコレクションの他のクイック スタート チュートリアルは、このクイック スタートに基づいています。

> [!TIP]
> 引き続きクイック スタート チュートリアルの作業を行う場合は、このクイック スタートで作成したリソースをクリーンアップしないでください。 これ以上作業を行わない場合は、次の手順に従って、このクイック スタートで作成したすべてのリソースを Azure Portal で削除してください。
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>次の手順

- これで、データベースが作成されたので、任意のツールまたは言語を使用して[接続し、クエリを実行](sql-database-connect-query.md)できます。
- 初めてのデータベースを設計し、テーブルを作成して、データを挿入する方法については、次のいずれかのチュートリアルを参照してください。
 - [SSMS を使用した最初の Azure SQL データベースの設計](sql-database-design-first-database.md)
  - [C# と ADO.NET で Azure SQL データベースを設計し、接続する](sql-database-design-first-database-csharp.md)

