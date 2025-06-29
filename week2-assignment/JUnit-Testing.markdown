# JUnit Setup and Assertions Guide

## How to Set Up JUnit
To begin writing unit tests, follow these steps:
1. Create a new Java project in your IDE.
2. If using Maven, add this to your `pom.xml`:
   ```xml
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.13.2</version>
       <scope>test</scope>
   </dependency>
   ```
![Setup image](<WhatsApp Image 2025-06-29 at 19.08.51_e29dcc6b.jpg>)

## Assertions in JUnit
Assertions verify your code's behavior. Example `User` class:
```java
import java.util.regex.Pattern;

public class User {
    public boolean isValidUsername(String username) {
        return username != null && username.length() >= 3 && username.matches("^[a-zA-Z0-9_]+$");
    }

    public boolean isValidEmail(String email) {
        if (email == null) return false;
        String regex = "^[\\w.-]+@[\\w.-]+\\.\\w{2,}$";
        return Pattern.matches(regex, email);
    }

    public boolean isStrongPassword(String password) {
        if (password == null || password.length() < 8) return false;
        return password.matches(".[A-Z].") &&
               password.matches(".[a-z].") &&
               password.matches(".\\d.") &&
               password.matches(".[!@#$%^&()].*");
    }
}
```
Test class with assertions:
```java
import static org.junit.Assert.assertFalse;
import static org.junit.Assert.assertTrue;
import org.junit.Test;

public class UserTest {
    private final User user = new User();

    @Test
    public void testValidUsername() {
        assertTrue(user.isValidUsername("Rahul_121"));
        assertTrue(user.isValidUsername("Cherry121"));
    }

    @Test
    public void testValidEmail() {
        assertTrue(user.isValidEmail("chavarahul4@gmail.com"));
        assertTrue(user.isValidEmail("cherry4@gmail.com"));
    }

    @Test
    public void testStrongPassword() {
        assertTrue(user.isStrongPassword("Rahul@123"));
        assertFalse(user.isStrongPassword("cherry"));
    }
}
```

![assertion](<WhatsApp Image 2025-06-29 at 19.09.52_2122554e.jpg>)

## AAA Pattern and Test Fixtures
Use the Arrange-Act-Assert pattern:
- **Arrange**: Prepare objects (e.g., `user`).
- **Act**: Call the method to test.
- **Assert**: Check the result.

Setup and teardown example:
```java
import static org.junit.Assert.assertFalse;
import static org.junit.Assert.assertTrue;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

public class UserTest {
    public User user;

    @Before
    public void setUp() {
        user = new User();
    }

    @After
    public void Finish() {
        user = null;
    }

    @Test
    public void testValidUsername() {
        assertTrue(user.isValidUsername("Rahul_121"));
        assertTrue(user.isValidUsername("Cherry121"));
    }

    @Test
    public void testValidEmail() {
        assertTrue(user.isValidEmail("chavarahul4@gmail.com"));
        assertTrue(user.isValidEmail("cherry4@gmail.com"));
    }

    @Test
    public void testStrongPassword() {
        assertTrue(user.isStrongPassword("Rahul@123"));
        assertFalse(user.isStrongPassword("cherry"));
    }
}
```

![alt text](<WhatsApp Image 2025-06-29 at 19.16.14_624250fc.jpg>)