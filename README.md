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

    public IWebElement FindElement(By locator)
    {
        try
        {
            IWebElement element = wait.Until(ExpectedConditions.ElementExists(locator));
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
            IWebElement element = FindElement(locator);
            element.Click();
            Console.WriteLine($"Clicked on element: {locator}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error clicking on element: {ex.Message}");
            throw;
        }
    }

    public void AssertElementExists(By locator)
    {
        try
        {
            FindElement(locator);
            Console.WriteLine($"Assertion passed: Element exists - {locator}");
        }
        catch (WebDriverTimeoutException ex)
        {
            Console.WriteLine($"Assertion failed: Element does not exist - {locator}");
            throw new AssertionException($"Element does not exist - {locator}", ex);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error asserting element existence: {ex.Message}");
            throw;
        }
    }

    public void AssertElementTextEquals(By locator, string expectedText)
    {
        try
        {
            IWebElement element = FindElement(locator);
            string actualText = element.Text;
            if (actualText.Equals(expectedText))
            {
                Console.WriteLine($"Assertion passed: Element text is '{expectedText}' - {locator}");
            }
            else
            {
                Console.WriteLine($"Assertion failed: Element text is '{actualText}', expected '{expectedText}' - {locator}");
                throw new AssertionException($"Element text is '{actualText}', expected '{expectedText}' - {locator}");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error asserting element text: {ex.Message}");
            throw;
        }
    }

    public void Close()
    {
        driver.Quit();
        Console.WriteLine("Browser closed");
    }
}

Sub GenerateAndIncrementNumbers()
    Dim ws As Worksheet
    Set ws = ThisWorkbook.Sheets("Sheet1") 'Change "Sheet1" to your sheet's name
    Dim startCell As Range
    Set startCell = ws.Range("A1") 'Change "A1" to your starting cell
    
    Dim currentCell As Range
    Dim currentNumber As Long
    currentNumber = 1 ' Initial number to start with
    
    ' Loop through cells in column A
    For Each currentCell In ws.Range(startCell, ws.Cells(ws.Rows.Count, startCell.Column).End(xlUp))
        ' Check if "SIT" is found on the right-hand side (RHS) of the current cell value
        If InStr(1, currentCell.Offset(0, 1).Value, "SIT", vbTextCompare) > 0 Then
            currentNumber = currentNumber + 1
        End If
        ' Set the number in the current cell
        currentCell.Value = currentNumber
    Next currentCell
End Sub





