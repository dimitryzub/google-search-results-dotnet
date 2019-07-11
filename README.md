# Google/Bing/Baidu Search Result in Dotnet / CSharp / .Net

[![Build Status](https://travis-ci.org/serpapi/google-search-results-dotnet.svg?branch=master)](https://travis-ci.org/serpapi/google-search-results-dotnet)
[![NuGet version](https://badge.fury.io/nu/google-search-results-dotnet.svg)](https://badge.fury.io/nu/google-search-results-dotnet)

This Dotnet package is meant to scrape and parse Google or Bing or Baidu results using [SerpApi](https://serpapi.com).

This extension is in development. But the code can be re-use for production because the API is already stable.

The following services are provided:
 * [Search API](https://serpapi.com/search-api) 
 * [Location API](https://serpapi.com/locations-api) - not implemented
 * [Search Archive API](https://serpapi.com/search-archive-api)  - not implemented
 * [Account API](https://serpapi.com/account-api) - not implemented

Serp API provides a [script builder](https://serpapi.com/demo) to get you started quickly.

Feel free to fork this repository to add more backends.

## Installation

To install the package.
```bash
dotnet add package google-search-results-dotnet --version 1.2.0
```

More commands available: [[https://www.nuget.org/packages/google-search-results-dotnet]]

## Quick start 

Search Google (default search engine)

```csharp
using System;
using GoogleSearchResults;
using System.Net.Http;
using Newtonsoft.Json.Linq;
using System.Collections;
using System.Threading.Tasks;
using System.Text.RegularExpressions;

class Program
{

  static void Main(string[] args)
  {
    // secret api key from https://serpapi.com/dashboard
    String apiKey = "";

    // Localized search for Coffee shop in Austin Texas
    Hashtable ht = new Hashtable();
    ht.Add("location", "Austin, Texas, United States");
    ht.Add("q", "Coffee");
    ht.Add("hl", "en");
    ht.Add("google_domain", "google.com");

    try
    {
      GoogleSearchResultsClient client = new GoogleSearchResultsClient(ht, apiKey);
      JObject data = client.GetJson();

      Console.WriteLine("local coffee shop");
      JArray coffeeShops = (JArray)data["local_results"];
      foreach (JObject coffeeShop in coffeeShops)
      {
        Console.WriteLine("Found: " + coffeeShop["title"]);
      }

      Console.WriteLine("organic result coffee shop");
      coffeeShops = (JArray)data["organic_results"];
      foreach (JObject coffeeShop in coffeeShops)
      {
        Console.WriteLine("Found: " + coffeeShop["title"]);
      }
    }
    catch (GoogleSearchResultsException ex)
    {
      Console.WriteLine("Exception:");
      Console.WriteLine(ex.ToString());
    }
  }
}
```

This example displays the top 3 coffee shop in Austin Texas found in the local_results.
Then it displays all 10 coffee shop found in the regular google search named: organic_results.

Note: If you like to search with Baidu or Bing.
```csharp
string engine = "bing"; # baidu
client =  new GoogleSearchResultsClient(ht, apiKey, engine);
```

TODO
---
 * [x] Add test
 * [x] Implement all 4 API
 * [x] Enable CI integration
 * [x] Publish package
 * [ ] Improve documentation
 * [ ] Add advanced examples like: https://github.com/serpapi/google-search-results-ruby (wait for user feedback)
