with testNG
package jar;

import java.time.Duration;

import org.openqa.selenium.Alert;
import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterClass;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

public class test {

    WebDriver driver;

    @BeforeClass
    public void setup() {

        System.setProperty("webdriver.chrome.driver",
                "E:\\Downloads\\chromedriver-win64\\chromedriver-win64\\chromedriver.exe");

        driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.manage().timeouts()
                .pageLoadTimeout(Duration.ofSeconds(10));
    }

    @Test
    public void studentManagementTest() throws InterruptedException {

        // Launch Website
        driver.get("http://localhost/student");
        Thread.sleep(5000);

        // Open Admin Page
        driver.get("http://localhost/student/admin");
        Thread.sleep(5000);

        // Login
        driver.findElement(By.name("username"))
                .sendKeys("admin");

        driver.findElement(By.name("password"))
                .sendKeys("admin@123");

        driver.findElement(By.xpath("//button[@type='submit']"))
                .click();

        System.out.println("Login Success");

        Thread.sleep(5000);

        // Student Menu
        driver.findElement(By.linkText("Students")).click();

        Thread.sleep(5000);

        // URL Validation
        String expectedURL =
                "http://localhost/student/admin/student.php";

        String currentURL = driver.getCurrentUrl();

        if (currentURL.equals(expectedURL)) {

            System.out.println("URL Validation Success");

        } else {

            System.out.println("URL Validation Fail");
        }

        Thread.sleep(5000);

        // Search Student
        WebElement search =
                driver.findElement(By.xpath("//input[@type='search']"));

        search.sendKeys("Ad");

        Thread.sleep(5000);

        // Add New Student
        WebElement addBtn =
                driver.findElement(By.id("new_std_btn"));

        ((JavascriptExecutor) driver)
                .executeScript("arguments[0].click();", addBtn);

        Thread.sleep(5000);

        driver.findElement(By.name("firstname"))
                .sendKeys("Stanley");

        Thread.sleep(2000);

        driver.findElement(By.name("lastname"))
                .sendKeys("Lambert");

        Thread.sleep(2000);

        driver.findElement(By.name("grade"))
                .sendKeys("Grade 11");

        Thread.sleep(2000);

        driver.findElement(By.name("phone"))
                .sendKeys("0777889090");

        Thread.sleep(2000);

        driver.findElement(By.name("save"))
                .click();

        Thread.sleep(5000);

        // Save Alert
        Alert addAlert = driver.switchTo().alert();

        System.out.println(
                "Student Saving Alert Message : "
                        + addAlert.getText());

        addAlert.accept();

        Thread.sleep(5000);

        // Delete Student
        WebElement deleteBtn = driver.findElement(
                By.xpath("(//button[contains(@class,'remove_student')])[1]"));

        ((JavascriptExecutor) driver)
                .executeScript("arguments[0].click();", deleteBtn);

        Thread.sleep(5000);

        Alert deleteAlert = driver.switchTo().alert();

        System.out.println(
                "Student Deleting Alert Message : "
                        + deleteAlert.getText());

        deleteAlert.dismiss();

        Thread.sleep(5000);

        // Update Student
        WebElement editBtn = driver.findElement(
                By.xpath("(//button[contains(@class,'edit_student')])[1]"));

        ((JavascriptExecutor) driver)
                .executeScript("arguments[0].click();", editBtn);

        Thread.sleep(5000);

        WebElement lname =
                driver.findElement(By.name("lastname"));

        lname.clear();

        Thread.sleep(2000);

        lname.sendKeys("User");

        Thread.sleep(2000);

        driver.findElement(
                By.xpath("//button[@class='btn btn-primary' and @name='save']"))
                .click();

        Thread.sleep(5000);

        Alert updateAlert = driver.switchTo().alert();

        System.out.println(
                "Student Updating Alert Message : "
                        + updateAlert.getText());

        updateAlert.accept();

        Thread.sleep(5000);

        // Logout
        driver.get("http://localhost/student/admin/logout.php");

        Thread.sleep(5000);

        String logoutURL = driver.getCurrentUrl();

        System.out.println("Current URL after logout: " + logoutURL);

        if (logoutURL.contains("index.php")
                || driver.findElements(By.name("username")).size() > 0) {

            System.out.println("Logout Success");

        } else {

            System.out.println("Logout Fail");
        }
    }

    @AfterClass
    public void closeBrowser() {

        driver.quit();
    }
}
