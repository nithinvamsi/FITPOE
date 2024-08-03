To execute the Java Selenium WebDriver script, follow these steps:

Set Up Your Environment:

Ensure you have JDK installed on your system. If not, download and install it from here.
Download and set up Maven from here.
Set Up Project Structure:

Create a new directory for your project, e.g., fitpeo-automation.
Inside this directory, create a src/main/java folder and a src/test/java folder.
Create pom.xml:

In the root of your project directory, create a pom.xml file with the following content:

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>fitpeo-automation</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.1.2</version>
        </dependency>
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.4.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>


package com.example.fitpeo;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class FitPeoAutomation {
    public static void main(String[] args) {
        // Set the path to the chromedriver executable
        System.setProperty("webdriver.chrome.driver", "path/to/chromedriver");  // Update with your ChromeDriver path

        WebDriver driver = new ChromeDriver();
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

        try {
            // Navigate to FitPeo Homepage
            driver.get("https://www.fitpeo.com");  // Update with the actual URL

            // Navigate to the Revenue Calculator Page
            WebElement revenueCalculatorLink = wait.until(ExpectedConditions.elementToBeClickable(By.linkText("Revenue Calculator")));
            revenueCalculatorLink.click();

            // Scroll Down to the Slider section
            WebElement sliderSection = wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("slider-section")));  // Use actual ID or locator
            ((org.openqa.selenium.JavascriptExecutor) driver).executeScript("arguments[0].scrollIntoView(true);", sliderSection);

            // Adjust the Slider to 820
            WebElement slider = driver.findElement(By.xpath("//*[@id='slider']"));  // Use actual XPath or locator
            Actions moveSlider = new Actions(driver);
            moveSlider.clickAndHold(slider).moveByOffset(820, 0).release().perform();

            // Update the Text Field to 560
            WebElement textField = driver.findElement(By.xpath("//*[@id='text-field']"));  // Use actual XPath or locator
            textField.click();
            textField.clear();
            textField.sendKeys("560");

            // Validate Slider Value
            Thread.sleep(1000);  // Wait for the slider to update
            String sliderValue = slider.getAttribute("value");
            if (!sliderValue.equals("560")) {
                throw new AssertionError("Slider value did not update to 560");
            }

            // Select CPT Codes
            String[] cptCodes = {"CPT-99091", "CPT-99453", "CPT-99454", "CPT-99474"};
            for (String code : cptCodes) {
                WebElement checkbox = driver.findElement(By.xpath("//input[@value='" + code + "']"));
                if (!checkbox.isSelected()) {
                    checkbox.click();
                }
            }

            // Validate Total Recurring Reimbursement
            WebElement totalReimbursement = driver.findElement(By.id("total-reimbursement"));  // Use actual ID or locator
            String totalReimbursementValue = totalReimbursement.getText();
            if (!totalReimbursementValue.equals("$110700")) {
                throw new AssertionError("Total Recurring Reimbursement value is incorrect");
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // Close the WebDriver
            driver.quit();
        }
    }
}


Update ChromeDriver Path:

Ensure the path to the chromedriver executable is correct in the script. Replace "path/to/chromedriver" with the actual path.
Build and Run the Script:

Open a terminal or command prompt and navigate to your project directory.
Use Maven to build and run the project:
bash
mvn clean compile exec:java -Dexec.mainClass="com.example.fitpeo.FitPeoAutomation"

Element Locators: Ensure the URL and element locators (e.g., IDs, XPaths) match the actual FitPeo website.
Dynamic Elements: Handle potential dynamic loading of elements and adjust wait times accordingly.
Browser-Specific Issues: Address any browser-specific issues or WebDriver configurations as needed.
By following these steps, you should be able to set up, compile, and run the Java Selenium WebDriver script to automate the task on the FitPeo website.







