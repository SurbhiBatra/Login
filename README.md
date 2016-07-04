# LoginModule

package com.vimond.automation.tests;

import static java.util.concurrent.TimeUnit.SECONDS;
import static org.junit.Assert.assertEquals;

import java.io.File;
import java.io.IOException;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.concurrent.TimeUnit;

import org.junit.AfterClass;
import org.junit.BeforeClass;
import org.junit.FixMethodOrder;
import org.junit.Test;
import org.junit.runners.MethodSorters;
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.NoSuchElementException;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.remote.RemoteWebDriver;
import org.openqa.selenium.support.PageFactory;
import org.openqa.selenium.support.ui.FluentWait;
import org.openqa.selenium.support.ui.Wait;
import org.openqa.selenium.support.ui.WebDriverWait;

import com.google.common.base.Function;
import com.gurock.testrail.APIException;
import com.vimond.automation.PageObject.LoginPageObj;

@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class LoginModule {

	public static final String USERNAME = "henriklarsen2";
	public static final String AUTOMATE_KEY = "SCWQPTYLgpdTUqJ4B6ha";
	public static final String URL = "https://" + USERNAME + ":" + AUTOMATE_KEY
			+ "@hub.browserstack.com/wd/hub";
	private static DesiredCapabilities caps = new DesiredCapabilities();
	private static WebDriver driver;
	static String driverPath = "\\chromedriver_win32\\chromedriver.exe";
	static boolean isRemoteExecution = false;

	LoginPageObj LoginPageObj;

	@BeforeClass
	public static void openBrowser() throws MalformedURLException {

		if (isRemoteExecution) {
			caps.setCapability("browser", "Chrome");
			caps.setCapability("browser_version", "50");
			caps.setCapability("os", "Windows");
			caps.setCapability("os_version", "10");
			caps.setCapability("browserstack.debug", "true");
			caps.setCapability("resolution", "1920x1080");

			driver = new RemoteWebDriver(new URL(URL), caps);
		} else {
			String parentPath = new File(System.getProperty("user.dir"))
					.getParentFile().getAbsolutePath();
			driverPath = parentPath + driverPath;
			System.setProperty("webdriver.chrome.driver", driverPath);
			driver = new ChromeDriver();
		}

		driver.manage().window().maximize();
		driver.manage().timeouts().implicitlyWait(5, TimeUnit.SECONDS);

		// Login Flow
		driver.get("https://vcc.ha.mbc-second.vimondtv.com");

		WebDriverWait wait = new WebDriverWait(driver, 5);
	}

	/*
	 * //Login flow
	 */
	
	@Test
	public void invalidUserLoginTest() throws IOException, APIException,
			InterruptedException {
		LoginPageObj loginPageObj = PageFactory.initElements(driver,
				LoginPageObj.class);
		// get Fields
		WebElement usernameField = loginPageObj.getUserNameField();
		WebElement passwordField = loginPageObj.getPasswordField();
		WebElement signInButton = loginPageObj.getLoginButton();
		System.out.println("Login using Invalid credentials");
		Thread.sleep(2000);
		// Empty USername with valid password
		usernameField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getEmpty());
		passwordField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getValidPassword());
		Thread.sleep(2000);
		signInButton.click();
		
		
		Thread.sleep(2000);
        // invalid username with valid password
        usernameField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getInvalidUserName());
		passwordField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getValidPassword());
		Thread.sleep(2000);
		signInButton.click();
		WebElement messageText = fluentWait(By.cssSelector("div.material-design-input-wrapper > span > div.error-message"));
        assertEquals("Username or password invalid", messageText.getText());
		
	}
	
	@Test
	public void invalidPasswordLoginTest() throws IOException, APIException,
			InterruptedException {
		LoginPageObj loginPageObj = PageFactory.initElements(driver,
				LoginPageObj.class);
		// get Fields
		WebElement usernameField = loginPageObj.getUserNameField();
		WebElement passwordField = loginPageObj.getPasswordField();
		WebElement signInButton = loginPageObj.getLoginButton();
		System.out.println("Login using Invalid credentials");
		Thread.sleep(2000);
		// Empty Password with valid Username
		usernameField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getValidUserName());
		passwordField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getEmpty());
		Thread.sleep(2000);
		signInButton.click();
		Thread.sleep(2000);
       
        // invalid password with valid username
        usernameField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getValidUserName());
		passwordField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getInvalidPassword());
		Thread.sleep(2000);
		signInButton.click();
		WebElement messageText = fluentWait(By.cssSelector("div.material-design-input-wrapper > span > div.error-message"));
        assertEquals("Username or password invalid", messageText.getText());
        
	}
	
	@Test
	public void invalidLoginTest() throws IOException, APIException,
			InterruptedException {
		LoginPageObj loginPageObj = PageFactory.initElements(driver,
				LoginPageObj.class);
		// get Fields
		WebElement usernameField = loginPageObj.getUserNameField();
		WebElement passwordField = loginPageObj.getPasswordField();
		WebElement signInButton = loginPageObj.getLoginButton();
		System.out.println("Login using Invalid credentials");
		Thread.sleep(2000);
		usernameField.sendKeys(Keys.chord(Keys.CONTROL, "a"), loginPageObj.getInvalidUserName());
		passwordField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getInvalidPassword());
		Thread.sleep(2000);
		signInButton.click();
		WebElement messageText = fluentWait(By.cssSelector("div.material-design-input-wrapper > span > div.error-message"));
        assertEquals("Username or password invalid", messageText.getText());
		
	}
	
	
	@Test
	public void validLoginTest() throws IOException, APIException,
			InterruptedException {
		LoginPageObj loginPageObj = PageFactory.initElements(driver,
				LoginPageObj.class);
		// get Fields
		WebElement usernameField = loginPageObj.getUserNameField();
		WebElement passwordField = loginPageObj.getPasswordField();
		WebElement signInButton = loginPageObj.getLoginButton();
		// login using valid credentials
		System.out.println("Login using Valid credentials");
		Thread.sleep(1000);
		usernameField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getValidUserName());
		Thread.sleep(1000);
		passwordField.sendKeys(Keys.chord(Keys.CONTROL, "a"),loginPageObj.getValidPassword());
		Thread.sleep(2000);
		signInButton.click();
		WebElement loggedInUsername = fluentWait(By.cssSelector("a.pluggable-user-email.edit-user-action"));
		String actualText = loginPageObj.getValidUserName();
        assertEquals(actualText, loggedInUsername.getText());

	}

	// End of login flow
	
	

	@AfterClass
	public static void closeBrowser() {
		System.out.println("Closing Chrome browser");
		driver.quit();
	}
	public static WebElement fluentWait(final By locator) {

        Wait<WebDriver> fluentWait = new FluentWait<WebDriver>(driver)
                .withTimeout(30, SECONDS)
                .pollingEvery(5, SECONDS)
                .ignoring(NoSuchElementException.class);

        WebElement elementFound = fluentWait.until(new Function<WebDriver, WebElement>() {
            public WebElement apply(WebDriver webDriver) {
                return driver.findElement(locator);
            }
        });

        return elementFound;
    }

}
