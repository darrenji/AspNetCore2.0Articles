> Rendering HTML

Rendering HTML from View Page is no different than rendering HTML in a normay way. HTML in the view page is rendered by the server unchanged.

> Razor syntax

use the `@` symbol to transition from HTML to C#. Razor engine evaluates C# expressions and renders them in the HTML output. In common situations, @ symbol will be translated into plain c#, but if @ symbol is followed by a Razor reserved keyword, in this sitiation, whe paragraph will be translated into Razor-specific markup.

> escape @ symbol

```
<p>@@Username</p>
```

> email中的@不会被看做razor的@

```
<a href="mailto:Suppoort@contoso.com">Support@constoso.com</a>
```

> 正常的@用法

```
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

> await的用法

```
@await DoSth()
```

> @不能直接使用泛型方法

```
//以下写法是错误的
@GenericMethod<int>()

//泛型方法必须放在explicit razor expression或razor code block
```

> @()的值会被显然出来

```
@(DateTime.Now - TimeSpan.FromDays(7))

//这种写法是explicit expression

@{
	var joe = new Person("joe",33);
}

<p>Age@(joe.Age)</p>

//以上，没有这个(),就会被看做是email

//以下是implicit expresson
//implicit中的<>会被看做HTML标签
//所以以下写法是错误的
<p>@GenericMethod<int>()</p>

//但以下写法是正确的
<p>@(GenericMethod<int>())</p>
```

> expression encoding

c#有时会被转换成html,这个过程是html encoded.在内部c# expression会被转换成IHtmlContect, 再调用IHtmlContent.WriteTo方法渲染成html string.

```
//以下直接转换成html string
//<span>Hello World</span>
@("<span>Hello World</span>")

//<span>Hello World</span>
@Html.Raw("<span>Hello World</span>");
```

> code blocks

code block中的内容不会被渲染。

```
@{
	var quote = "";
}

<p>@quote</p>

//也可以让code block中的内容渲染成html

@{
	var inCSharp = true;
	<p>Now in HTML, was in c# @inCSharp</p>
}

//
@for(var i = 0; i < people.Length; i++){
	var person = peopele[i];
	<text>Name:@person.Name</text>
}
```

> @if, else i, else, and @switch

```
@if(value % 2 ==0){
	<p>The value was even.</p>
}
else i(value >= 1337){
	<p>The value is large.</p>
}
else 
{
	<p></p>
}

//
@switch(value)
{

}
```

> 循环

```
@for(var i = 0; i< people.Length; i++) {
	var person = people[i];
	<p>Name: @person.Name</p>
	<p>Age: @person.Age</p>
}

@foreach(var person in people)

@{ var i = 0;}
@while(i < people.Length)

@{var i = 0;}
@do{

} whitle (i < people.Length)
```

> @using

```
@using(Html.BeginForm()){}
```

> try catch finally

```
@try{}
catch(Exception ex)
finally{}
```

> lock

```
@lock(SomeLock){
	//
}
```

> 注释

```
/* dd */
// commment
<!--comment-->

@*

*@
```

> 引用空间

```
@using System.IO
```

> 引用模型

```
@model TypeNameOfModel
```

> @inherits的用法, 引用自定义视图类，和@model很像

```
using Microsoft.AspNetCore.Mvc.Razor;

public abstract class CustomRazorPage<TModel> : RazorPage
{
	public string CustomText {get;} = "";
}

@inherits CustomRazorPage<TModel>
<div>@CustomText</div>
```

> @injext


注入服务

> functions

```
@functions{
	public string GetHello(){
		return "Hello";
	}
}

<div>@GetHello()</div>
```

> section

```
@RenderSection("Scripts", required:false)

@section Scripts {
	<script type="text/javascript" src="" />
}
```
