Test tasks for Automation Engineer Additional Information: BASE_URL for testing https://cc.healthrecoverysolutions.com/login 
1. What page element selectors do you know?
ID
Name
Class
TagName
LinkText
PartialLinkText
CSS
Xpath
2. Rate your JS knowledge from 0 to 10 
Ans:
I  have some basic knowledge about JS, if the project is based on JS I am willing to learn JS .

3. Rate your Cypress knowledge from 0 to 10 
Ans:
I do not have any working experience with Cypress however I have worked with Selenium.

4. Use selectors to get a list of all admins 
<ul class="users_list"> 
<li class="admin">one</li> 
<li class="user">two</li> 
<li class="admin">three</li> 
<li class="user">four</li> 
</ul> 

Ans:
List<WebElement> listOfElements = driver.findElements(By.Class("admin"));

5. Use selectors to list all users by data attribute name="user" 
<ul class="users_list"> 
<li class="admin">one</li> 
<li name="user"">two</li> 
<li class="admin">three</li> 
<li name="user">four</li> 
</ul> 

Ans:
List<WebElement> listOfElements = driver.findElements(By.Name("user"));

6. Filter the list by color attribute. The condition is that you first get the complete list of elements and then filter on the attribute. You need to get a list of items with a red marker. 
<ul class="users_list"> 
<li class="user" color=”red”>test1</li> 
<li class="user" color=”green”>test2</li> 
<li class="user" color=”red”>test3</li> 
<li class="user" color=”green”>test4</li> 
<li class="user" color=”red”>test5</li> 
<li class="user" color=”green”>test6</li> 
<li class="user" color=”green”>test7</li> 
<li class="user" color=”red”>test8</li> 
<li class="user" color=”green”>test9</li> 
<li class="user" color=”red”>test10</li> 
<li class="user" color=”green”>test11</li> 
</ul> 
Ans:
List<WebElement> listOfElements = driver.findElements(By.Xpath("//*[@class='user'and color=’red’]”));

7. What actions for elements do you know? 
	driver.FindBy(By.xpath(“ Xpath”)).click();
driver.FindBy(By.CSS(“ css selector”)).submit();
driver.FindBy(By.id(“ id locator”)).getText();
driver.FindBy(By.class(“ class locator”)).sendKeys();


8. How to get an element using Cypress? 
Ans:
In Selenium 
findElement: A command used to uniquely identify a web element within the web page.

9. How to work with select in Cypress? 

Select objSelect = new Select(driver.findElement(By.id(Element_ID)));
objSelect.selectByIndex(index);
// Or can be used as
objSelect.selectByVisibleText(text);
// Or can be used as
objSelect.selectByValue(value);

10. What actions can be applied to an element using Cypress?
 Ans:
Mouse Actions:
doubleClick(): Performs double click on the element
clickAndHold(): Performs long click on the mouse without releasing it
dragAndDrop(): Drags the element from one point and drops to another
moveToElement(): Shifts the mouse pointer to the center of the element
contextClick(): Performs right-click on the mouse
Keyboard Actions:
sendKeys(): Sends a series of keys to the element
keyUp(): Performs key release
keyDown(): Performs keypress without release

11. Does cypress support drag and drop? 
Ans:
Yes, Selenium supports drag and drop. To achieve drag and drop we need to use the Actions class.

Actions action = new Actions(driver);
action.dragAndDrop(SourceElement, TargetElement).build().perform();


12. Does cypress support file uploads? How? 
Ans:
Selenium support file uploads. 

WebElement chooseFile = driver.findElement(By.id("custom-input"));
chooseFile.sendKeys("/Users/myDocument/Downloads/file1.png");

13. Practical task[UI] 
Open the above link in your browser. Write test cases (cases must be complete in terms of assertions). Implement these cases with Cypress and the Page Objects pattern. The weblink must be used with cypress.env.json 
● Add some scripts to package.json: 
1. Opening Cypress UI 
2. Running tests in a specific browser. 
● In on("before:browser:launch", (browser = {}, launchOptions) => {}); add additional launch options ○ incognito 
○ fullscreen 
Ans:
package common;
public class CommonClass {
   @BeforeMethod
      public void setUp(@Optional("https://www.google.com") String url){
       System.setProperty("webdriver.chrome.driver", "path for the chrome.exe file");
       Webdriver driver = new ChromeDriver();
       driver.get("https://cc.healthrecoverysolutions.com");
       driver.manage().window().maximize();
       driver.manage().timeouts().pageLoadTimeout(20, TimeUnit.SECONDS);
       driver.manage().timeouts().implicitlyWait(30,TimeUnit.SECONDS);
   }

   @AfterMethod(alwaysRun = true)
   public void cleanUp() {
       driver.quit();
   }
}
-----------------------------------------------------
Package home;
public class HrsHome {
@FindBy(id = "loginEmail")WebElement userNameField;
@FindBy(xpath = "//input[@id='password']")WebElement userPasswordField;
@FindBy(id = "loginSubmitButton")WebElement logInButton;

public void hrsAccountLogIn(){
   userNameField.sendKeys("useremail@gmail.com");
   userPasswordField.sendKeys("Abc123");
   logInButton.click();
}
-------------------------------------------------------------------------------
package testHome;

public class TestLogIn extends CommonClass {
   HrsHome  hrsHome;
   @BeforeMethod
   public void getInIt(){
       hrsHome= PageFactory.initElements(driver, HrsHome.class);
   }
   @Test
   public void testHrsAccountLogIn(){
      hrsHome.hrsAccountLogIn();
Assert.assertEquals("actual title", driver.getTitle());
   }
}


14. Practical task[API-> Rest-Assured]
Write tests for API based on previously created cases. Tests can be written linearly. Add data models and assertions to your tests. Take base URL, base path, and endpoints from the browser console. In the ResponseSpecification, add a code status check and a content type check. Map the server response to the object. 

Answer:
package test_hrs;

public class HrsAPI {
   protected String baseUrl="https://cc.healthrecoverysolutions.com”;
   protected String endpoints="/login.json”;
   protected String apiSecretKey="apiSecretKey";
   protected String apiKey="apiKey”;
   protected String accessToken="accessToken”;
   protected String accessTokenSecret="accessTokenSecret”;

   public ValidatableResponse hrsLogIn() {
       return given().auth().oauth(apiKey, apiSecretKey, accessToken, accessTokenSecret)
               .when().get(baseUrl + endpoints)
               .then().assertThat().statusCode(200).
                       and().contentType(ContentType.JSON).
                       and().header("Content",equalTo("login"));
   }

}

15. Submit your test assignments to GitHub. Projects must be public.

