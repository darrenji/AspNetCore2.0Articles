放在了Pages目录中。

	Pages
		_Layout.cshtml
		_ValidationScritpsPartial.cshtml
		_ViewImports.cshtml
		_ViewStart.cshtml

当请求过来的时候，ASP.NET Core 2.0会到Pages目录中来找对应的Razor Page.

	/或/Index对应/Pages/Index.cshtml
	/Contact对应Pages/Contact.cshtml
	/Store/Contact对应Pages/Store/Contact.cshtml

来看一个Razor Page的视图页， About.cshtml：

	@page  //一定要放在最前面，告诉运行时来找About.cshtml.cs文件
	@model AboutModel
	@{
		ViewData["Title"]="";
	}

这里又出现一个问题。当请求过来的时候，直接到Razor Page来要内容了，原来的Controller去了哪里呢？这个和AboutModel相关。

	//PageModel中包含了Controller中的很多特性
	public class AboutModel : PageModel
	{
		public string Message{get;set;}
		public void OnGet()
		{
			Message = "";
		}
	}

	//以上等同于
	public IActionResult About()
	{
		ViewData["Message"]="";
		return View();
	}

还可以把About.cshtml.cs中的内容放到Razor Page上来。当要让下面的代码运行，需要删除About.cshtml.cs文件或注释掉里面的代码。否则运行时，当执行到Razor Page上的@function的时候会优先执行About.cshtml.cs。

	@page
	@funcitons{
		public string Message{get;set;}
		
		public void OnGet()
		{
			Message= "";
		}
	}
	@{
		ViewData["title"] = "about";
	}