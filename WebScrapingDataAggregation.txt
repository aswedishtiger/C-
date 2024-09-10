using System;
using System.IO;
using HtmlAgilityPack;
using System.Collections.Generic;

class WebScraper
{
    static List<string[]> ScrapeData(string url)
    {
        HtmlWeb web = new HtmlWeb();
        HtmlDocument doc = web.Load(url);
        
        List<string[]> data = new List<string[]>();

        foreach (HtmlNode node in doc.DocumentNode.SelectNodes("//your_xpath"))
        {
            string[] row = new string[] { node.InnerText };
            data.Add(row);
        }

        return data;
    }

    static void WriteToCsv(List<string[]> data, string filePath)
    {
        using (StreamWriter writer = new StreamWriter(filePath))
        {
            foreach (var row in data)
            {
                writer.WriteLine(string.Join(",", row));
            }
        }
    }