首先是控制器

```
public class SpeakerController : Controller
{
	public List<ModelData> Speakers = new List<odelData>{};

	[Route("Speaker/{id:int}")]
	public IActionResult Detail(int id)
	{
		return View(Speakers.FirstOrDeault(a => a.SpeakerId ==id));
	}

	[Route("/Speakder/Evaluations", Name = "speakerevals")]
	public IActionResult Evaluations()
	{
		return View();
	}

	[Route("/Speaker/EvaluationsCurrent", Name="speakerevalscurrent")]
	public IActionResult EvaluationsCurrent(string speakerId, string currentYear)
	{
		return View();
	}

	public IActionResult Index()
	{
		return View(Speakers);
	}
}

public class ModelData
{
	public int SpeakerId{get;set;}
}
```

用法

```
<a asp-controller="Speaker" asp-action="Index">All Speaker</a>

```

> asp-route-{value}

```
public IActionResult AnchorTagHelper(string id)
{
	var speaker = new SpeakerData(){SpeakerId = 12};
	return View(viewName, speaker);
}

@model SpeakerData

<a asp-controller="" asp-action="" asp-route-id=@Model.SpeakerId></a>
```

> asp-all-route-data

```
@{
	var dit = new Dictionary<string, string>{};
}

<a asp-route="speakerevalscurrent" asp-all-route-data="dict"></a>
```

> asp-fragmentt

```
<a asp-action="Evaluations" asp-controller="Speaker" asp-fragment="SpeakerEvaluations"><a>

//
/Speaker/Evaluations#SpeakerEvaluations
```

> asp-area

```
<a asp-action="" asp-controller="" asp-area="">dd</a>
```

> asp-protocol

```
<a asp-protocol="https" asp-action="" asp-controller=""></a>
```

> cache tag helper

```
<cache enabled="true" expires-after="@DateSpan.FromSeconds(120)" vary-by-heaer="User-Agent" vary-by-query="" vary-by-route="" vary-by-cookie="" vary-by-user="">
	@Dateime.Now
<cache>
```

> image tag helper

```
<img src="" asp-append-version="true" />
```