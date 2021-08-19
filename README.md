# YoutubeSearchApi.Net
![CodeFactor Grade](https://img.shields.io/codefactor/grade/github/madeyoga/YoutubeSearchApi.Net?style=for-the-badge)
[![Nuget](https://img.shields.io/nuget/dt/Discord.Addons.Music?color=GREEN&style=for-the-badge)](https://www.nuget.org/packages/YoutubeSearchApi.Net/)
[![contributionswelcome](https://img.shields.io/badge/contributions-welcome-brightgreen/?style=for-the-badge)](https://github.com/madeyoga/YoutubeSearchApi.Net/issues)
[![discord_invite](https://img.shields.io/discord/458296099049046018?style=for-the-badge)](https://discord.gg/Y8sB4ay)

A lightweight & simple .NET library to extract data from Youtube.
The purpose of this project is to make it easier for developers to extract data from YouTube or any sources.


## Supported Sites
- Youtube
- Youtube Music

## Installation

### NuGet
- [YoutubeSearchApi.Net](https://www.nuget.org/packages/YoutubeSearchApi.Net/)


## Quick Start
**Youtube Search**
```C#
using YoutubeSearchApi.Net;
using YoutubeSearchApi.Net.Backends;
using YoutubeSearchApi.Net.Objects;

public static async Task AsyncMain()
{
    HttpClient httpClient = new HttpClient();

    DefaultSearchClient client = new DefaultSearchClient(new YoutubeSearchBackend());

    YoutubeResponse responseObject = (YoutubeResponse) await client.SearchAsync(httpClient, "black suit", maxResults: 5);

    Console.WriteLine("RESPONSE: ");
    foreach(YoutubeVideo video in responseObject.Results)
    {
        Console.WriteLine(video.ToString());
        Console.WriteLine("");
    }

    httpClient.Dispose();
}

public static void Main(string[] args)
{
    AsyncMain().GetAwaiter().GetResult();
}
```

**Youtube Music Search**
```C#
using YoutubeSearchApi.Net;
using YoutubeSearchApi.Net.Backends;
using YoutubeSearchApi.Net.Objects;

public static async Task AsyncMain()
{
    HttpClient httpClient = new HttpClient();

    string key = Environment.GetEnvironmentVariable("YT_MUSIC_KEY");
    DefaultSearchClient client = new DefaultSearchClient(new YoutubeMusicSearchBackend(key));

    var response = (YoutubeResponse) await client.SearchAsync(httpClient, "black suit", maxResults: 5);

    foreach (YoutubeVideo video in response.Results)
    {
        Console.WriteLine(video);
    }

    httpClient.Dispose();
}

public static void Main(string[] args)
{
    AsyncMain().GetAwaiter().GetResult();
}
```

Full [examples at Test project](https://github.com/madeyoga/YoutubeSearchApi.Net/tree/master/Tests)

## Implements Your Own Search Client
Implements `ISearchBackend` interface:

```C#
public class MySearchBackend : ISearchBackend 
{
    public class MySearchBackend() 
    {
    
    }

    public IResponseObject ParseData(string pageContent, int maxResults)
    {
        throw new NotImplementedException();
    }

    public async Task<string> RequestDataAsync(HttpClient httpClient, string query, int retry = 3, Dictionary<string, object> extras = null)
    {
        throw new NotImplementedException();
    }
}
```

Use it with `DefaultSearchClient`:
```C#
var client = new DefaultSearchClient(new MySearchBackend());
```


## Code Review
I tried implementing the Dependency Inversion Principle in this project. Code review would really help me out :]


## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
