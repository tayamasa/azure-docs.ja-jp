---
title: Azure Stack での SQL データベースの使用 | Microsoft Docs
description: SQL データベースを Azure Stack にサービスとしてデプロイする方法と、SQL Server リソース プロバイダー アダプターの簡単なデプロイ手順について説明します。
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 55d0e51606e8768a01c0b5a7766dbafe24d97a0d
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307827"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Microsoft Azure Stack で SQL データベースを使用する

SQL Server リソースプロバイダー アダプター API を使用して、SQL データベースを [Azure Stack](azure-stack-poc.md) のサービスとして公開します。 リソースプロバイダーをインストールして 1 つまたは複数の SQL Server インスタンスに接続すると、御社および御社のユーザーは次のものを作成できます。

- クラウドネイティブ アプリ向けデータベース。
- SQL を使用する Websites。
- SQL を使用するワークロード。

リソース プロバイダーでは、[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) のすべてのデータベース管理機能が提供されているわけではありません。 たとえば、リソースを自動的に割り当てるエラスティック プールはサポートされていません。 ただし、リソース プロバイダーでは、SQL Server データベースに対して同様の作成、読み取り、更新、および削除 (CRUD) 操作がサポートされています。 リソース プロバイダー API の詳細については、「[Windows Azure Pack SQL Server Resource Provider REST API Reference](https://msdn.microsoft.com/library/dn528529.aspx)」(Windows Azure Pack SQL Server リソース プロバイダー REST API リファレンス) を参照してください。

>[!NOTE]
SQL Server リソース プロバイダー API は、Azure SQL Database と互換性がありません。

## <a name="sql-resource-provider-adapter-architecture"></a>SQL リソースプロバイダー アダプターのアーキテクチャ

リソース プロバイダーは、次のコンポーネントで構成されています。

- **SQL リソース プロバイダーの仮想マシン (VM)**。プロバイダー サービスを実行する Windows Server VM です。
- **リソース プロバイダー**。要求を処理し、データベース リソースにアクセスします。
- **SQL Server をホストするサーバー**。これはデータベースに容量を提供し、ホスティング サーバーと呼ばれます。

少なくとも 1 つの SQL Server インスタンスを作成するか、外部 SQL Server インスタンスへのアクセスを提供する必要があります。

> [!NOTE]
> Azure Stack 統合システムにインストールされたホスティング サーバーは、テナント サブスクリプションから作成する必要があります。 既定のプロバイダー サブスクリプションからは作成できません。 作成するには、適切なサインインで、テナント ポータルまたは PowerShell を使用する必要があります。 すべてのホスティング サーバーが課金対象の仮想マシンであり、ライセンスを所有している必要があります。 サービス管理者は、テナント サブスクリプションの所有者になることができます。

## <a name="next-steps"></a>次の手順

[SQL Server リソース プロバイダーのデプロイ](azure-stack-sql-resource-provider-deploy.md)
