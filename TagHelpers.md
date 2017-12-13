> what are tag helpers?

enable server-side code to participate in creating and rendering HTML eleents.

```
<label asp-for="Email"></label>
```

如果项目的名称是AuthoringTagHelpers, Views/_ViewImports.cshtml

```
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper,AuthoringTagHelpers
```

Views/SomeName/_ViewImports.cshtml，在某个视图页移除TagHelpers

```
@removeTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper,AuthoringTagHelpers
```

> 禁用tag helper

```
<!span asp-validation-for="Email"><!/span>
```

> 添加前缀

Views/_ViewImports.cshtml
```
@tagHelperPrefix th:
```

> Tag Helpers的智能提示

```
Microsoft.AspNetCore.Razor.Tools
```

> 常见的表达

```
<form asp-controller="" asp-action="" method="post" class="orm-horizontal">
	<div asp-validation-summary="All" class="text-dagner">

	<div class="form-group">
		<label asp-for="Email" class="col-md-2 control-label"></label>
		<div class="col-md-10">
			<input asp-for="Email" class="form-control" />
			<span asp-validation-for="Email" class="text-danger"></span>
		</div>
	</div>
	<div class="form-group">
		<div class="col-md-offset-2 col-md-10">
			<button type="submit" class="btn btn-default">Regiser</button>
		</div>
	</div>
</form>
```

> 自己写一个Tag Helper

```
using Microsoft.AspNetCore.Razor.TagHelpers;
using System.Threading.Tasks;

namespace AuthoriingTagHelpers : TagHeleper
{
	public class EmailTagHelper : TagHelper
	{
		public override void Process(TagHelperContext context, TagHelperOutput outpu)
		{
			output.TagName = "a";
		}
	}
}
```

Views/_ViewImports.cshtml

```
@using AuthoringTagHelpers
@addTagHelper *,Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper *, AuthoringTagHelpers
```

使用

```
<email>Support</email>
```

改善，加上属性

```
public class EmailTagHelper : TagHelper
{
	private const string EmailDomain = "";
	public string MailTo{get;set;}

	public override void Process()
	{
		output.TagName = "a";

		var address = MailTo + "@" + EmailDomain;
		output.Attrbutes.SetAttribute("href","mailto:" + address );
		output.Content.SetContent(address);
	}
}
```

更新视图中

```
<email mail-to=""></email>
```

再更新自定义的TagHelper

```
[HtmlTargetElement("email", TagStructure = TagStructure.WithoutEndTag)]
public class EmailVoidTagHelper : TagHelper
```

> 异步的TagHelper

```
public class EmailTagHelper  : TagHelper
{
	private const string EmailDomain = "contoso.com";
	public override async Task ProcessAsync()
	{
		output.TagName = "a";
		var content = await output.GetChildContentAsync();
		var target = content.GetContect() + "@" + EmailDomain;
		output.Attributes.SetAttribute("href", "mailto:" + target);
		output.Content.SetContent(target);
	}
}
```

> 再来一个TagHelper

```
[HtmlTargetElement(Attributes = "bold")]
[HtmlTargetElement("bold", Attributes = "bold")]
public class BoldTagHelper : TagHelper
{
	public override void Process()
	{
		output.Attributes.RemoveAll("bold");
		output.PreContent.SetHtmlContent("<strong>");
		output.PostContent.SetHtmlContent("</strong>");
	}
}
```

使用
```
<p bold>...</p>
```

> 把一个模型传给TagHelper

```
public class WebsiteContext
{
	public Version Verson
	public int CopyrightYear
	public bool Approved
	public int TagsToShow
}
```

TagHelper

```
	[HtmlTargetElement("WebsiteInfomration")]
	public class WebsiteInormationTagHelper : TagHelper
	{
		public WebsiteContext Info{get;set;}

		public override void Process()
		{
			output.TagName = "section";
			output.Content.SetHtmlContent();

			output.TagMode = TagMode.StartTagAndEnd;
		}
	}
```

使用

```
@using AhthoringTagHelpers.Models
@{
	ViewData["Title"] = "";
}

<website-information info ="new WebsiteContenxt{}" />
```

> Tag Helpers in forms

```

//这样写会默认产生一个有关verificaitontoken的隐藏域
<form asp-controller="" asp-action="" method="">
	 <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>


//路由
<form asp-controller="" asp-action="" asp-route-returnurl="@ViewData["ReturnUrl"]" method="post" class="form-horizontal" role="form">

</form>


```

> 嵌套导航属性

```
public class AddressViewModel
{
	public string AddressLine1{get;set;}
}

public class RegisterAddressViewModel
{
	public string Email
	public string Password
	public AddressViewModel Address
}
```

在视图中

```
@model RegisterAddressViewModel

<form asp-controller="" asp-action="" method="post">
	<input asp-for="Email" />
	<input asp-for="Password" />
	<input asp-for="Address.AddressLine1">
	<button type="submit">Register</button>
</form>

//
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" >
```

> 处理集合属性

```
public class Person 
{
	public List<string> Colors{get;set;}

	public int Age{get;se;t}
}

public IActionResult Edit(int d, int colorIndex)
{
	ViewData["Index"] = colorIndex;
	return View(GetPerson(id));
}

@model Person
@{
	var index = (int)ViewData["index"];
}

<form asp-controller="" asp-action="" method="">
	@Html.EditorFor(m => m.Colors[index])
	<label asp-for="Age"></label>
	<input asp-for="Age">
	<buton type=""></button>
</form>

Vews/Shared/EdtiorTemplates/String.cshtml

@model string
<label asp-for="@Model"></label>
<input asp-for="@Model"></label>

```

> 处理集合

```
public class ToDoItem
{
	public string Name
	public bool IsDone
}

@model List<ToDoItem>

<form asp-controller="" asp-action="" method="">
	@for(int i = 0; i< ModelCOunt; i++){
		@Htm.EditorFor(model => model[i])
	}
</form>

//Viess/Shared/EdtiorTemplates/ToDoItem.cshtml

@model ToDoItem4

<label asp-for="@Model.Name"></label>
```

> Textarea

```
<textarea asp-for=""/>
```

> Validation 

```
<span asp-validation-for="Email" />
```

> validation summary tag helper

```
<div asp-validation-summary="">
```

> select

```
<select asp-for="" asp-items = "Model.Countires"

public List<SelectListItem> Countiries {get;} = new List<SelectListItem>{
	new SelectListItem{}
};
```

> 枚举,这个很好

```
public class CountryEnumViewModel
{
	public CountryEnum EnumCOuntry{get;set;}
}

public enum CountryEnum
{
	[Display(Name="")]
	Meico,
	USa
}

@model COuntryEnumViewModel
<form>
	<select asp-for="EnumCOuntry" asp-items="Html.GetEnumSelectList<CountryEnum>()">

	</select>
</form>
```

