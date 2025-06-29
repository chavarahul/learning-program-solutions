# Mocking, Stubbing, and Interaction Verification Guide

## Mocking and Stubbing
Mock external dependencies using Mockito. Example `ExternalApi` and `MyService`:
```java
package com.example.mocking;

public interface ExternalApi {
    String getData();
}

package com.example.mocking;

public class MyService {
    private final ExternalApi externalApi;

    public MyService(ExternalApi externalApi) {
        this.externalApi = externalApi;
    }

    public String fetchData() {
        return externalApi.getData();
    }
}
```
Test with stubbing:
```java
package com.example.mocking;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.mockito.Mockito.*;

public class MyServiceTest {
    @Test
    public void testExternalApi() {
        ExternalApi mockApi = mock(ExternalApi.class);
        when(mockApi.getData()).thenReturn("Mock Data");

        MyService service = new MyService(mockApi);
        String result = service.fetchData();

        assertEquals("Mock Data", result);
    }
}
```

![alt text](<WhatsApp Image 2025-06-29 at 20.02.09_fb30b2f7.jpg>)

## Verifying Interactions
Check if methods are called using `verify`. Example `Messenger` class:
```java
package com.example.mocking;

public class Messenger {
    private final ExternalApi api;

    public Messenger(ExternalApi api) {
        this.api = api;
    }

    public void sendMessage() {
        api.getData();
    }
}
```
Test to verify interaction:
```java
package com.example.mocking;

import org.junit.jupiter.api.Test;
import static org.mockito.Mockito.*;

public class MessengerTest {
    @Test
    public void testInteraction() {
        ExternalApi mockApi = mock(ExternalApi.class);

        Messenger messenger = new Messenger(mockApi);
        messenger.sendMessage();

        verify(mockApi).getData();
    }
}
```

![alt text](<WhatsApp Image 2025-06-29 at 20.03.19_ed557f26.jpg>)