# Api_UI_Automation_Framework_PythonBDD
This repository hosts a comprehensive automation framework designed for API and UI testing, built using Python and Behave. The framework provides a scalable and efficient solution for automating tests across different layers of your application stack, enabling seamless integration of API and UI testing in your development workflow.


using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.Support.UI;
using System;

public class SeleniumHelper
{
    private IWebDriver driver;
    private WebDriverWait wait;

    public SeleniumHelper()
    {
        // Initialize WebDriver (in this example, using Chrome)
        driver = new ChromeDriver();
        // Initialize WebDriverWait with a timeout of 10 seconds
        wait = new WebDriverWait(driver, TimeSpan.FromSeconds(10));
    }

    public void NavigateToUrl(string url)
    {
        try
        {
            driver.Navigate().GoToUrl(url);
            Console.WriteLine($"Navigated to URL: {url}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error navigating to URL: {ex.Message}");
            throw;
        }
    }

    public T FindElement<T>(By locator) where T : IWebElement
    {
        try
        {
            T element = wait.Until(driver => driver.FindElement(locator) as T);
            Console.WriteLine($"Element found: {locator}");
            return element;
        }
        catch (WebDriverTimeoutException ex)
        {
            Console.WriteLine($"Timeout waiting for element: {locator}");
            throw new TimeoutException($"Timeout waiting for element: {locator}", ex);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error finding element: {ex.Message}");
            throw;
        }
    }

    public void Click(By locator)
    {
        try
        {
            IWebElement element = FindElement<IWebElement>(locator);
            element.Click();
            Console.WriteLine($"Clicked on element: {locator}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error clicking on element: {ex.Message}");
            throw;
        }
    }

    public void WriteText(By locator, string text)
    {
        try
        {
            IWebElement element = FindElement<IWebElement>(locator);
            element.Clear();
            element.SendKeys(text);
            Console.WriteLine($"Entered text '{text}' into element: {locator}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error writing text: {ex.Message}");
            throw;
        }
    }

    public void WaitForElement(By locator)
    {
        try
        {
            wait.Until(driver => driver.FindElement(locator));
            Console.WriteLine($"Element waited for: {locator}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error waiting for element: {ex.Message}");
            throw;
        }
    }

    public void Close()
    {
        driver.Quit();
        Console.WriteLine("Browser closed");
    }
}
