---
title: "条件付きステートメント - 条件に基づいてステップを実行する - Azure Logic Apps | Microsoft Docs"
description: "条件を満たした後にのみロジック アプリのステップを実行します。 指定された条件に基づいてワークフローを実行するデシジョン ツリーを作成します。"
services: logic-apps
keywords: "条件付きステートメント、デシジョン ツリー"
documentationcenter: 
author: ecfan
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 486c1053f42ed3becc2c4b60accc993db7f24baa
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2018
---
# <a name="conditional-statements-run-steps-based-on-a-condition-in-logic-apps"></a>条件付きステートメント: ロジック アプリで条件に基づいてステップを実行する

指定された条件を満たした後でのみステップを実行するには、"*条件付きステートメント*" を使用します。 この構造体は、ワークフローのデータを特定の値またはフィールドと比較します。 その後、データが条件を満たしたかどうかによって実行するさまざまなステップを定義できます。 条件は入れ子にすることができます。

たとえば、Web サイトの RSS フィードに新しい項目が出現したときに送信される電子メールが多すぎるロジック アプリがあるとします。 条件付きステートメントを追加して、新しい項目に特定の文字列が含まれる場合のみ電子メールを送信することができます。 

> [!TIP]
> いくつかの特定の値に基づいてさまざまなステップを実行する場合は、[*switch ステートメント*](../logic-apps/logic-apps-control-flow-switch-statement.md)を使用します。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。 サブスクリプションをお持ちでない場合には、[無料の Azure アカウントにサインアップ](https://azure.microsoft.com/free/)してください。

* [ロジック アプリの作成方法](../logic-apps/quickstart-create-first-logic-app-workflow.md)に関する基本的な知識

* この記事の例に従うには、Outlook.com や Office 365 の Outlook アカウントを使用して、[このサンプルのロジック アプリを作成](../logic-apps/quickstart-create-first-logic-app-workflow.md)します。

## <a name="add-a-condition"></a>条件を追加する

1. <a href="https://portal.azure.com" target="_blank">Azure Portal</a> のロジック アプリ デザイナーでロジック アプリを開きます。

2. 必要な場所に条件を追加します。 

   ステップの間に条件を追加するには、条件を追加する位置の矢印の上にポインターを移動します。 表示される**プラス記号** (**+**) を選択し、**[条件の追加]** を選択します。 例: 

   ![ステップ間に条件を追加する](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   ワークフローの末尾に条件を追加するときは、ロジック アプリの一番下で **[+ 新しいステップ]** > **[条件の追加]** を選択します。

3. **[条件]** で、条件を作成します。 

   1. 左のボックスに、比較するデータまたはフィールドを指定します。

      **[動的なコンテンツの追加]** リストで、自分のロジック アプリの既存のフィールドを選択できます。

   2. 中央のリストで、実行する操作を選択します。 
   3. 右のボックスに、条件として値またはフィールドを指定します。

   例: 

   ![基本モードで条件を編集する](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode.png)

   完成した条件を次に示します。

   ![完成した条件](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode-2.png)

   > [!TIP]
   > 高度な条件を作成したり、式を使用したりするには、**[Edit in advanced mode]\(詳細モードで編集\)** を選択します。 [ワークフロー定義言語](../logic-apps/logic-apps-workflow-definition-language.md)で定義された式を使用できます。
   > 
   > 例: 
   >
   > ![コードで条件を編集する](./media/logic-apps-control-flow-conditional-statement/edit-condition-advanced-mode.png)

5. **IF YES** と **IF NO** の下に、条件を満たすかどうかに基づいて実行するステップを追加します。 例: 

   ![YES および NO のパスの条件](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > 既存のアクションを **IF YES** と **IF NO** のパスにドラッグできます。

6. ロジック アプリを保存し、

これで、このロジック アプリは、RSS フィードの新しい項目が条件を満たす場合のみメールを送信します。

## <a name="json-definition"></a>JSON の定義

条件付きステートメントを使用してロジック アプリを作成しました。ここで、条件付きステートメントの背後にあるコード定義の概要を見てみましょう。

``` json
"actions": {
  "myConditionName": {
    "type": "If",
    "expression": "@contains(triggerBody()?['summary'], 'Microsoft')",
    "actions": {
      "Send_an_email": {
        "inputs": { },
        "runAfter": {}
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>サポートを受ける

* 質問がある場合は、[Azure Logic Apps フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)にアクセスしてください。
* 機能や提案について投稿や投票を行うには、[Azure Logic Apps のユーザー フィードバック サイト](http://aka.ms/logicapps-wish)にアクセスしてください。

## <a name="next-steps"></a>次の手順

* [さまざまな値に基づいてステップを実行する (switch ステートメント)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [ステップを実行して繰り返す (ループ)](../logic-apps/logic-apps-control-flow-loops.md)
* [並列ステップを実行または結合する (分岐)](../logic-apps/logic-apps-control-flow-branches.md)
* [グループ化されたアクションの状態に基づいてステップを実行する (スコープ)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)