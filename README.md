import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import java.io.File;
import java.io.IOException;
import org.apache.commons.io.FileUtils;

public class SignUpAutomationTest {

    public static void main(String[] args) {
        // Set the path of the ChromeDriver executable
        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");

        WebDriver driver = new ChromeDriver();

        driver.get("https://tutorialsninja.com/demo/index.php?route=account/login");

        // Test Case 1 - Successful Sign Up
        WebElement firstNameField = driver.findElement(By.id("input-firstname"));
        firstNameField.sendKeys("John");

        WebElement lastNameField = driver.findElement(By.id("input-lastname"));
        lastNameField.sendKeys("Smith");

        WebElement emailField = driver.findElement(By.id("input-email"));
        emailField.sendKeys("john.smith@example.com");

        WebElement passwordField = driver.findElement(By.id("input-password"));
        passwordField.sendKeys("Pass1234");

        WebElement confirmPasswordField = driver.findElement(By.id("input-confirm"));
        confirmPasswordField.sendKeys("Pass1234");

        WebElement termsCheckbox = driver.findElement(By.name("agree"));
        termsCheckbox.click();

        WebElement signUpButton = driver.findElement(By.cssSelector("input[type='submit'][value='Continue']"));
        signUpButton.click();

        // Capture screenshot for verification
        captureScreenshot(driver, "SignUp_Successful");

        // Test Case 2 - Missing First Name
        firstNameField.clear();
        lastNameField.clear();
        emailField.clear();
        passwordField.clear();
        confirmPasswordField.clear();

        lastNameField.sendKeys("Smith");
        emailField.sendKeys("john.smith@example.com");
        passwordField.sendKeys("Pass1234");
        confirmPasswordField.sendKeys("Pass1234");
        termsCheckbox.click();
        signUpButton.click();

        // Capture screenshot for verification
        captureScreenshot(driver, "SignUp_MissingFirstName");

        // Test Case 3 - Password Mismatch
        firstNameField.sendKeys("John");
        passwordField.sendKeys("Pass1234");
        confirmPasswordField.sendKeys("Pass5678");
        signUpButton.click();

        // Capture screenshot for verification
        captureScreenshot(driver, "SignUp_PasswordMismatch");

        // Test Case 4 - Invalid Email Format
        emailField.clear();
        emailField.sendKeys("john.smith.example.com");
        confirmPasswordField.sendKeys("Pass1234");
        signUpButton.click();

        // Capture screenshot for verification
        captureScreenshot(driver, "SignUp_InvalidEmailFormat");

        // Close the browser
        driver.quit();
    }

    // Method to capture screenshot
    public static void captureScreenshot(WebDriver driver, String screenshotName) {
        try {
            // Convert WebDriver object to TakesScreenshot
            TakesScreenshot screenshot = (TakesScreenshot) driver;

            // Call getScreenshotAs method to create a file
            File sourceFile = screenshot.getScreenshotAs(OutputType.FILE);

            // Define the destination path and file name
            String destinationPath = "path/to/screenshot/folder/" + screenshotName + ".png";

            // Copy the file to the destination path
            FileUtils.copyFile(sourceFile, new File(destinationPath));

            System.out.println("Screenshot captured: " + screenshotName);
        } catch (IOException e) {
            System.out.println("Error capturing screenshot: " + e.getMessage());
        }
    }
}
