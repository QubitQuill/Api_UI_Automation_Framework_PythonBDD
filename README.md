# Api_UI_Automation_Framework_PythonBDD
This repository hosts a comprehensive automation framework designed for API and UI testing, built using Python and Behave. The framework provides a scalable and efficient solution for automating tests across different layers of your application stack, enabling seamless integration of API and UI testing in your development workflow.


using OpenQA.Selenium;
using OpenQA.Selenium.Support.UI;
using System;

public class SeleniumHelper
{
    private IWebDriver driver;
    private WebDriverWait wait;

    public SeleniumHelper(IWebDriver driver)
    {
        this.driver = driver;
        // Initialize WebDriverWait with a timeout of 10 seconds
        wait = new WebDriverWait(driver, TimeSpan.FromSeconds(10));
    }

    public T FindElement<T>(string pageName, By locator, LocatorType locatorType) where T : class
    {
        try
        {
            IWebElement element = null;
            switch (locatorType)
            {
                case LocatorType.Id:
                    element = wait.Until(ExpectedConditions.ElementExists(By.Id(locator.ToString())));
                    break;
                case LocatorType.Name:
                    element = wait.Until(ExpectedConditions.ElementExists(By.Name(locator.ToString())));
                    break;
                // Add cases for other locator types as needed
                default:
                    throw new ArgumentException($"Unsupported locator type: {locatorType}");
            }

            // Convert the found IWebElement to the desired type T
            T convertedElement = (T)Convert.ChangeType(element, typeof(T));
            Console.WriteLine($"Element found on page '{pageName}': {locator}");
            return convertedElement;
        }
        catch (NoSuchElementException ex)
        {
            Console.WriteLine($"Element not found on page '{pageName}': {locator}");
            throw new NoSuchElementException($"Element not found on page '{pageName}': {locator}", ex);
        }
        catch (TimeoutException ex)
        {
            Console.WriteLine($"Timeout waiting for element on page '{pageName}': {locator}");
            throw new TimeoutException($"Timeout waiting for element on page '{pageName}': {locator}", ex);
        }
        catch (ArgumentException ex)
        {
            Console.WriteLine($"Unsupported locator type: {locatorType}");
            throw new ArgumentException($"Unsupported locator type: {locatorType}", ex);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error finding element on page '{pageName}': {ex.Message}");
            throw new Exception($"Error finding element on page '{pageName}': {ex.Message}", ex);
        }
    }
}

public enum LocatorType
{
    Id,
    Name,
    // Add more locator types as needed
}
