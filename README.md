# Api_UI_Automation_Framework_PythonBDD
This repository hosts a comprehensive automation framework designed for API and UI testing, built using Python and Behave. The framework provides a scalable and efficient solution for automating tests across different layers of your application stack, enabling seamless integration of API and UI testing in your development workflow.


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
