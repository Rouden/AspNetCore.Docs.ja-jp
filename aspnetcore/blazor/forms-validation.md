---
title: Blazor フォームと検証の ASP.NET Core
author: guardrex
description: Blazor でフォームとフィールドの検証シナリオを使用する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/forms-validation
ms.openlocfilehash: 0b2e38cdbd974a28960b917fb6b5ce370f8c4659
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030326"
---
# <a name="aspnet-core-blazor-forms-and-validation"></a>Blazor フォームと検証の ASP.NET Core

作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

[データ注釈](xref:mvc/models/validation)を使用して、Blazor でフォームおよび検証をサポートしています。

> [!NOTE]
> フォームと検証のシナリオは、プレビューリリースごとに変更される可能性があります。

次`ExampleModel`の型は、データ注釈を使用して検証ロジックを定義します。

```csharp
using System.ComponentModel.DataAnnotations;

public class ExampleModel
{
    [Required]
    [StringLength(10, ErrorMessage = "Name is too long.")]
    public string Name { get; set; }
}
```

フォームは、 `EditForm`コンポーネントを使用して定義されます。 次のフォームは、一般的な要素、コンポーネント、および Razor コードを示しています。

```csharp
<EditForm Model="@exampleModel" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <InputText id="name" @bind-Value="@exampleModel.Name" />

    <button type="submit">Submit</button>
</EditForm>

@code {
    private ExampleModel exampleModel = new ExampleModel();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

* フォームは、 `ExampleModel`型で定義さ`name`れた検証を使用して、フィールドのユーザー入力を検証します。 モデルはコンポーネントの`@code`ブロック内に作成され、プライベートフィールド (`exampleModel`) に保持されます。 フィールドは、 `Model` `<EditForm>`要素の属性に割り当てられます。
* コンポーネント`DataAnnotationsValidator`は、データ注釈を使用して検証サポートをアタッチします。
* コンポーネント`ValidationSummary`は、検証メッセージの概要を示します。
* `HandleValidSubmit`は、フォームが正常に送信された (検証に合格した) ときにトリガーされます。

ユーザー入力の受信と検証には、一連の組み込みの入力コンポーネントを使用できます。 入力は、変更されたときとフォームが送信されたときに検証されます。 次の表に、使用できる入力コンポーネントを示します。

| 入力コンポーネント | 表示形式&hellip;       |
| --------------- | ------------------------- |
| `InputText`     | `<input>`                 |
| `InputTextArea` | `<textarea>`              |
| `InputSelect`   | `<select>`                |
| `InputNumber`   | `<input type="number">`   |
| `InputCheckbox` | `<input type="checkbox">` |
| `InputDate`     | `<input type="date">`     |

を含む`EditForm`すべての入力コンポーネントは、任意の属性をサポートします。 コンポーネントパラメーターに一致しない属性は、表示される HTML 要素に追加されます。

入力コンポーネントは、編集時に検証を行い、フィールドの状態を反映するように CSS クラスを変更するための既定の動作を提供します。 一部のコンポーネントには、役に立つ解析ロジックが含まれています。 たとえば、と`InputDate` `InputNumber`は、検証エラーとして登録することによって、解析不能な値を適切に処理します。 Null 値を受け入れることができる型では、ターゲットフィールドの null 値の`int?`許容もサポートされます (たとえば、)。

次`Starship`の型では、以前`ExampleModel`よりも大きなプロパティとデータ注釈を使用して検証ロジックを定義しています。

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16, ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000, ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true", 
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

前の例では`Description` 、データ注釈が存在しないため、は省略可能です。

次のフォームは、 `Starship`モデルで定義されている検証を使用してユーザー入力を検証します。

```cshtml
@page "/FormsValidation"

<h1>Starfleet Starship Database</h1>

<h2>New Ship Entry Form</h2>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" 
            @bind-Value="starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>

    <p>
        <a href="http://www.startrek.com/">Star Trek</a>, 
        &copy;1966-2019 CBS Studios, Inc. and 
        <a href="https://www.paramount.com">Paramount Pictures</a>
    </p>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        Console.WriteLine("OnValidSubmit");
    }
}
```

は`EditForm` 、変更`EditContext`されたフィールドと現在の検証メッセージを含む、編集プロセスに関するメタデータを追跡する[カスケード値](xref:blazor/components#cascading-values-and-parameters)としてを作成します。 また`EditForm` 、は、有効かつ無効な送信 (`OnValidSubmit`, `OnInvalidSubmit`) の便利なイベントも提供します。 または、 `OnSubmit`を使用して、検証フィールドとチェックフィールドの値をカスタム検証コードでトリガーします。

コンポーネント`DataAnnotationsValidator`は、データ注釈をカスケード`EditContext`に使用して検証サポートをアタッチします。 現在、データ注釈を使用した検証のサポートを有効にするには、この明示的なジェスチャが必要ですが、この動作を既定の動作に設定することを検討しています。 データ注釈とは異なる検証システムを使用するには`DataAnnotationsValidator` 、をカスタム実装に置き換えます。 ASP.NET Core の実装は、参照ソースの検査に使用できます。[DataAnnotationsValidator](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/DataAnnotationsValidator.cs)/[AddDataAnnotationsValidation](https://github.com/aspnet/AspNetCore/blob/master/src/Components/Components/src/Forms/EditContextDataAnnotationsExtensions.cs)。 *ASP.NET Core の実装は、プレビューリリース期間中に迅速な更新が適用されます。*

コンポーネント`ValidationSummary`は、検証の[概要タグヘルパー](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)に似たすべての検証メッセージを要約します。

コンポーネント`ValidationMessage`には、[検証メッセージタグヘルパー](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)に似た、特定のフィールドの検証メッセージが表示されます。 `For`属性を使用した検証用のフィールドと、モデルプロパティに名前を付けるラムダ式を指定します。

```cshtml
<ValidationMessage For="@(() => starship.MaximumAccommodation)" />
```

コンポーネント`ValidationMessage` と`ValidationSummary`コンポーネントは、任意の属性をサポートします。 コンポーネントパラメーターに一致しない属性は、生成`<div>`される要素また`<ul>`は要素に追加されます。