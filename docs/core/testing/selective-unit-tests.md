---
title: 選択的単体テストの実行
description: dotnet test コマンドでフィルター式を使用して、選択的単体テストを実行する方法を紹介します。
author: smadala
ms.author: mairaw
ms.date: 03/22/2017
ms.openlocfilehash: caea91e9884dba6bc07a7ef6a99699e43d23399c
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="running-selective-unit-tests"></a>選択的単体テストの実行

次の例では、`dotnet test` を使用します。 `vstest.console.exe` を使用している場合は、`--filter ` を `--testcasefilter:` に置き換えます。

## <a name="mstest"></a>MSTest

```csharp
namespace MSTestNamespace
{
    using Microsoft.VisualStudio.TestTools.UnitTesting;

    [TestClass]
    public class UnitTestClass1
    {
        [Priority(2)]
        [TestMethod]
        public void TestMethod1()
        {
        }

        [TestCategory("CategoryA")]
        [Priority(3)]
        [TestMethod]
        public void TestMethod2()
        {
        }
    }
}
```

| 正規表現 | 結果 |
| ---------- | ------ |
| `dotnet test --filter Method` | `FullyQualifiedName` に `Method` が含まれるテストを実行します。 `vstest 15.1+` で使用できます。 |
| `dotnet test --filter Name~TestMethod1` | 名前に `TestMethod1` が含まれるテストを実行します。 |
| `dotnet test --filter ClassName=MSTestNamespace.UnitTestClass1` | クラス `MSTestNamespace.UnitTestClass1` 内にあるテストを実行します。<br>**注:** `ClassName` 値には名前空間があるため、`ClassName=UnitTestClass1` は機能しません。 |
| `dotnet test --filter FullyQualifiedName!=MSTestNamespace.UnitTestClass1.TestMethod1` | `MSTestNamespace.UnitTestClass1.TestMethod1` 以外のテストをすべて実行します。 |
| `dotnet test --filter TestCategory=CategoryA` | `[TestCategory("CategoryA")]` の注釈が付けられているテストを実行します。 |
| `dotnet test --filter Priority=3` | `[Priority(3)]` の注釈が付けられているテストを実行します。<br>**注:** `Priority~3` は文字列ではないため、無効な値です。 |

**条件演算子| と &amp; の使用**

| 正規表現 | 結果 |
| ---------- | ------ |
| <code>dotnet test --filter "FullyQualifiedName~UnitTestClass1&#124;TestCategory=CategoryA"</code> | `FullyQualifiedName` に `UnitTestClass1` がある、**または** `TestCategory` が `CategoryA` のテストを実行します。 |
| `dotnet test --filter "FullyQualifiedName~UnitTestClass1&TestCategory=CategoryA"` | `FullyQualifiedName` に `UnitTestClass1` がある、**および** `TestCategory` が `CategoryA` のテストを実行します。 |
| <code>dotnet test --filter "(FullyQualifiedName~UnitTestClass1&TestCategory=CategoryA)&#124;Priority=1"</code> | `UnitTestClass1` を含む `FullyQualifiedName` **および** `TestCategory` が `CategoryA`、**または** `Priority` が 1 かのいずれかのテストを実行します。 |

## <a name="xunit"></a>xUnit

```csharp
namespace XUnitNamespace
{
    public class TestClass1
    {
        [Trait("Category", "bvt")]
        [Trait("Priority", "1")]
        [Fact]
        public void foo()
        {
        }

        [Trait("Category", "Nightly")]
        [Trait("Priority", "2")]
        [Fact]
        public void bar()
        {
        }
    }
}
```

| 正規表現 | 結果 |
| ---------- | ------ |
| `dotnet test --filter DisplayName=XUnitNamespace.TestClass1.Test1` | `XUnitNamespace.TestClass1.Test1` という 1 つのテストのみを実行します。 |
| `dotnet test --filter FullyQualifiedName!=XUnitNamespace.TestClass1.Test1` | `XUnitNamespace.TestClass1.Test1` 以外のテストをすべて実行します。 |
| `dotnet test --filter DisplayName~TestClass1` | 表示名に `TestClass1` が含まれるテストを実行します。 |

コード例では、特性をキー `Category` や `Priority` で定義すると、フィルター処理に使用できます。

| 正規表現 | 結果 |
| ---------- | ------ |
| `dotnet test --filter XUnit` | `FullyQualifiedName` に `XUnit` が含まれるテストを実行します。  `vstest 15.1+` で使用できます。 |
| `dotnet test --filter Category=bvt` | `[Trait("Category", "bvt")]` があるテストを実行します。 |

**条件演算子| と &amp; の使用**

| 正規表現 | 結果 |
| ---------- | ------ |
| <code>dotnet test --filter "FullyQualifiedName~TestClass1&#124;Category=Nightly"</code> | `FullyQualifiedName` に `TestClass1` がある、**または** `Category` が `Nightly` のテストを実行します。 |
| `dotnet test --filter "FullyQualifiedName~TestClass1&Category=Nightly"` | `FullyQualifiedName` に `TestClass1` がある、**および** `Category` が `Nightly` のテストを実行します。 |
| <code>dotnet test --filter "(FullyQualifiedName~TestClass1&Category=Nightly)&#124;Priority=1"</code> | `TestClass1` を含む `FullyQualifiedName` **および** `Category` が `CategoryA`、**または** `Priority` が 1 かのいずれかのテストを実行します。 |
