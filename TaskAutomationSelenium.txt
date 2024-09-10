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