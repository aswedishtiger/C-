# C# Automation Projects

This repository contains C# automation scripts for various use cases, including file automation, email automation, web scraping, and browser task automation using Selenium. These projects can be easily extended and integrated into larger automation systems, making them great additions to any portfolio.

## Table of Contents
1. [File Organization Automation](#file-organization-automation)
2. [Web Scraping with C#](#web-scraping-with-c)
3. [Email Automation](#email-automation)
4. [Excel File Processing](#excel-file-processing)
5. [Task Automation with Selenium](#task-automation-with-selenium)
6. [Installation and Setup](#installation-and-setup)
7. [Usage](#usage)

---

## File Organization Automation

This C# program automatically organizes files in a specified directory by their file extension, creating folders for each extension and moving the files accordingly.

### Features
- Organizes files based on their extension.
- Automatically creates folders for different file types.

### Example Usage
```bash
dotnet run
```

### Sample Code
```csharp
using System;
using System.IO;

class FileOrganizer
{
    static void OrganizeFiles(string directoryPath)
    {
        DirectoryInfo directory = new DirectoryInfo(directoryPath);

        foreach (FileInfo file in directory.GetFiles())
        {
            string extension = file.Extension.TrimStart('.').ToUpper();
            string newFolder = Path.Combine(directoryPath, extension);

            if (!Directory.Exists(newFolder))
            {
                Directory.CreateDirectory(newFolder);
            }

            string newFilePath = Path.Combine(newFolder, file.Name);
            file.MoveTo(newFilePath);
        }
    }

    static void Main(string[] args)
    {
        string directoryPath = @"C:\path\to\your\directory";
        OrganizeFiles(directoryPath);
        Console.WriteLine("Files organized successfully.");
    }
}
```

---

## Web Scraping with C#

This C# program demonstrates how to scrape data from a website using **HtmlAgilityPack** and saves the scraped data into a CSV file.

### Features
- Scrapes data from a webpage.
- Saves the scraped data into a CSV file.

### Example Usage
```bash
dotnet run
```

### Sample Code
```csharp
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

    static void Main(string[] args)
    {
        string url = "https://example.com";
        string outputPath = @"C:\path\to\output.csv";
        
        List<string[]> data = ScrapeData(url);
        WriteToCsv(data, outputPath);
        
        Console.WriteLine("Data scraped and saved to CSV.");
    }
}
```

---

## Email Automation

This C# program automates sending emails using **System.Net.Mail** with the option to attach files.

### Features
- Send emails to a specified recipient.
- Attach files (e.g., PDFs, reports, etc.).
- Configure the email server and credentials securely.

### Example Usage
```bash
dotnet run
```

### Sample Code
```csharp
using System;
using System.Net;
using System.Net.Mail;

class EmailAutomation
{
    static void SendEmail(string recipient, string subject, string body, string attachmentPath = null)
    {
        MailMessage mail = new MailMessage();
        SmtpClient smtpServer = new SmtpClient("smtp.gmail.com");

        mail.From = new MailAddress("your_email@gmail.com");
        mail.To.Add(recipient);
        mail.Subject = subject;
        mail.Body = body;

        if (attachmentPath != null)
        {
            Attachment attachment = new Attachment(attachmentPath);
            mail.Attachments.Add(attachment);
        }

        smtpServer.Port = 587;
        smtpServer.Credentials = new NetworkCredential("your_email@gmail.com", "your_password");
        smtpServer.EnableSsl = true;

        smtpServer.Send(mail);
        Console.WriteLine($"Email sent to {recipient}.");
    }

    static void Main(string[] args)
    {
        string recipient = "recipient@example.com";
        string subject = "Automated Email";
        string body = "This is an automated email.";
        string attachmentPath = @"C:\path\to\file.pdf";
        
        SendEmail(recipient, subject, body, attachmentPath);
        Console.WriteLine("Emails sent successfully.");
    }
}
```

---

## Excel File Processing

This C# program uses **EPPlus** to read and merge multiple Excel files into a single output file.

### Features
- Merge data from multiple Excel files into one.
- Export the merged data to a new Excel file.

### Example Usage
```bash
dotnet run
```

### Sample Code
```csharp
using System;
using System.IO;
using OfficeOpenXml;
using System.Data;

class ExcelProcessor
{
    static void MergeExcelFiles(string directoryPath, string outputFilePath)
    {
        DataTable dataTable = new DataTable();

        foreach (var file in Directory.GetFiles(directoryPath, "*.xlsx"))
        {
            using (var package = new ExcelPackage(new FileInfo(file)))
            {
                ExcelWorksheet worksheet = package.Workbook.Worksheets[0];
                foreach (var row in worksheet.Cells[1, 1, worksheet.Dimension.Rows, worksheet.Dimension.Columns])
                {
                    dataTable.LoadDataRow(row.Value.ToString().Split(','), LoadOption.PreserveChanges);
                }
            }
        }

        using (var package = new ExcelPackage(new FileInfo(outputFilePath)))
        {
            ExcelWorksheet worksheet = package.Workbook.Worksheets.Add("MergedSheet");
            worksheet.Cells["A1"].LoadFromDataTable(dataTable, true);
            package.Save();
        }

        Console.WriteLine("Excel files merged successfully.");
    }

    static void Main(string[] args)
    {
        string directoryPath = @"C:\path\to\excel\files";
        string outputPath = @"C:\path\to\output.xlsx";

        MergeExcelFiles(directoryPath, outputPath);
    }
}
```

---

## Task Automation with Selenium

This C# program automates browser tasks like logging into a website using **Selenium**.

### Features
- Automate browser login.
- Interact with web elements (e.g., form filling, clicking buttons).
- Scrape data or download files after interaction.

### Example Usage
```bash
dotnet run
```

### Sample Code
```csharp
using System;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;

class SeleniumAutomation
{
    static void AutomateLogin()
    {
        IWebDriver driver = new ChromeDriver();
        driver.Navigate().GoToUrl("https://example.com/login");

        IWebElement usernameField = driver.FindElement(By.Name("username"));
        IWebElement passwordField = driver.FindElement(By.Name("password"));

        usernameField.SendKeys("your_username");
        passwordField.SendKeys("your_password");
        passwordField.Submit();

        Console.WriteLine("Login automated.");
        driver.Quit();
    }

    static void Main(string[] args)
    {
        AutomateLogin();
        Console.WriteLine("Browser task automation completed.");
    }
}
```

---

## Installation and Setup

### Prerequisites
- .NET Core SDK
- **Required NuGet Packages**:
  - `HtmlAgilityPack`: `Install-Package HtmlAgilityPack`
  - `EPPlus`: `Install-Package EPPlus`
  - `Selenium.WebDriver`: `Install-Package Selenium.WebDriver`
  - `Selenium.WebDriver.ChromeDriver`: `Install-Package Selenium.WebDriver.ChromeDriver`

### Installing Dependencies
You can install all the required dependencies for the C# automation projects by running the following commands:

```bash
dotnet add package HtmlAgilityPack
dotnet add package EPPlus
dotnet add package Selenium.WebDriver
dotnet add package Selenium.WebDriver.ChromeDriver
```

---

## Usage

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/csharp-automation-projects.git
   ```

2. **Navigate to the Project Directory**:
   ```bash
   cd csharp-automation-projects
   ```

3. **Run Individual Projects**:
   Use the following commands to run individual automation scripts:
   ```bash
   dotnet run
   ```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Feel free to customize and extend these automation scripts to suit your needs!
