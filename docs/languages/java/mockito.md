---
tags: [java, test, mockito, mock]
---
# MOCKITO

*Last update: 7 Oct 2022*

### Annotations

```java
@RunWith(MockitoJUnitRunner.class)

@Mock private XXX xxx;

@InjectMocks  private XXX sut;
```

### Capturing

```java
@Captor ArgumentCaptor<XXX> captorXxx;
@Captor ArgumentCaptor<AuctionEvent> arg;

captorXxx.capture()
captorXxx.getValue()
```

### Testing an abstract class

```java
@RunWith(MockitoJUnitRunner.class)
public class DerivatiProcessTest {

	private DerivatiProcess sut;

	@Before
	public void setUp() {
		sut = Mockito.mock(DerivatiProcess.class, Mockito.CALLS_REAL_METHODS);
	}
```	