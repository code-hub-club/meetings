# [Junit 5](https://junit.org/junit5/docs/current/user-guide/#overview)
## Junit = Platform + Jupiter + Vintage
- Junit Platform
    - `JVM`-д тест хийх framework-ийг эхлүүлэх суурь
    - Эхлүүлэгч болон `TestEngine API`
        - [TestEngine](https://junit.org/junit5/docs/current/api/org/junit/platform/engine/TestEngine.html) нь `JUnit Jupiter` хэл дээр бичигдсэн тестыг олож ажилуулах
    - [Console Launcher](https://junit.org/junit5/docs/current/user-guide/#running-tests-console-launcher), [Grandle](https://junit.org/junit5/docs/current/user-guide/#running-tests-build-gradle), [Maven](https://junit.org/junit5/docs/current/user-guide/#running-tests-build-maven) ашиглан тест эхлүүлэх боломжтой
- Junit Jupiter
    - Шинэ програм, өргөтгөл загварын хослол юм.
- Junit Vintange
    - Junit 3, 4-т зориулсан `TestEngine` юм.

## [Тест бичих](https://junit.org/junit5/docs/current/user-guide/#writing-tests)
## [Annotation](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)
`Junit Jupiter` нь дараах `annotation`-уудыг тест тохируулахад ашигладаг.

| Annotation           | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `@Test`              | Тест хийх функц гэж заана.                                   |
| `@ParameterizedTest` | Параметртэй тест хийхийг заана.                              |
| `@RepeatedTest`      | Давталттай тест хийхийг заана.                               |
| `@DisplayName`       | Тест класс болон функц-д нэр өгнө.                           |
| `@BeforeEach`        | Тухайн классын хүрээнд тест бүрийн өмнө дуудагдах функц.     |
| `@AfterEach`         | Тухайн классын хүрээнд тест бүрийн хойно дуудагдах функц.    |
| `@BeforeAll`         | Тухайн классын хүрээн дэх бүх тестийн өмнө дуудагдах функц.  |
| `@AfterAll`          | Тухайн классын хүрээн дэх бүх тестийн хойно дуудагдах функц. |
| `@Nested`            | Тест классын дотор тест класс байна.                         |
| `@Tag`               | Тестүүдийг ангилахад ашиглана.                               |
| `@Disabled`          | Тест класс болон функцыг зогсоох                             |

## Тест класс, функц
Тест функц нь `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`, `@TestTemplate` `annotation` доорх функцуудыг хэлнэ. Тест класс нь ядаж нэг тест функц агуулсан хамгийн дээд түвшиний класс.

```Java{.line-numbers}
import static org.junit.jupiter.api.Assertions.fail;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class StandardTests {

    @BeforeAll
    static void initAll() {
    }

    @BeforeEach
    void init() {
    }

    @Test
    void succeedingTest() {
    }

    @Test
    void failingTest() {
        fail("a failing test");
    }

    @Test
    @Disabled("for demonstration purposes")
    void skippedTest() {
        // not executed
    }

    @AfterEach
    void tearDown() {
    }

    @AfterAll
    static void tearDownAll() {
    }

}
```

## Display Name
Тест класс, функцид дурын нэр өгч болно.
```Java{.line-numbers}
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

@DisplayName("A special test case")
class DisplayNameDemo {

    @Test
    @DisplayName("Custom test name containing spaces")
    void testWithDisplayNameContainingSpaces() {
    }

    @Test
    @DisplayName("╯°□°）╯")
    void testWithDisplayNameContainingSpecialCharacters() {
    }

    @Test
    @DisplayName("😱")
    void testWithDisplayNameContainingEmoji() {
    }

}
```

## Assertions
`org.junit.jupiter.api.Assertions`
Ямар нэгэн юмыг үнэн гэж итгэхийг хэлнэ.

```Java{.line-numbers}
class AssertionsDemo {

    @Test
    void standardAssertions() {
        assertEquals(2, 2);
        assertEquals(4, 4, "The optional assertion message is now the last parameter.");
        assertTrue('a' < 'b', () -> {
            
            return "hi";
        });
    }

    @Test
    void groupedAssertions() {
        // In a grouped assertion all assertions are executed, and any
        // failures will be reported together.
        assertAll("person",
            () -> assertEquals("John", person.getFirstName()),
            () -> assertEquals("Doe", person.getLastName())
        );
    }
}
```

## Assumption
`org.junit.jupiter.api.Assumptions`
```Java{.line-numbers}
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assumptions.assumeTrue;
import static org.junit.jupiter.api.Assumptions.assumingThat;

import org.junit.jupiter.api.Test;

class AssumptionsDemo {

    @Test
    void testOnlyOnCiServer() {
        assumeTrue("CI".equals(System.getenv("ENV")));
        // remainder of test
    }

    @Test
    void testOnlyOnDeveloperWorkstation() {
        assumeTrue("DEV".equals(System.getenv("ENV")),
            () -> "Aborting test: not on developer workstation");
        // remainder of test
    }

    @Test
    void testInAllEnvironments() {
        assumingThat("CI".equals(System.getenv("ENV")),
            () -> {
                // perform these assertions only on the CI server
                assertEquals(2, 2);
            });

        // perform these assertions in all environments
        assertEquals("a string", "a string");
    }

}
```

## Тест зогсоох
`@Disabled` ашиглан бүх тест класс, функцыг зогсоож чадна. 
Тест класс зогсоох.
```Java{.line-numbers}
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

@Disabled
class DisabledClassDemo {
    @Test
    void testWillBeSkipped() {
    }
}
```
Тест функц зогсоох.
```Java{.line-numbers}
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class DisabledTestsDemo {

    @Disabled
    @Test
    void testWillBeSkipped() {
    }

    @Test
    void testWillBeExecuted() {
    }
}
```

## Tagging and Filtering
`@Tag`-ийн дүрэм
- Таг нь хоосон байж болохгүй.
- Таг нь зай зай агуулж болохгүй.
- Дараах тэмдэгт агуулж болохгүй
    - `,`, `(`, `)`, `&`, `|`, `!`
```Java{.line-numbers}
import org.junit.jupiter.api.Tag;
import org.junit.jupiter.api.Test;

@Tag("fast")
@Tag("model")
class TaggingDemo {

    @Test
    @Tag("taxes")
    void testingTaxCalculation() {
    }

}
```

## Nested Tests
```Java{.line-numbers}
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertThrows;
import static org.junit.jupiter.api.Assertions.assertTrue;

import java.util.EmptyStackException;
import java.util.Stack;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;

@DisplayName("A stack")
class TestingAStackDemo {

    Stack<Object> stack;

    @Test
    @DisplayName("is instantiated with new Stack()")
    void isInstantiatedWithNew() {
        new Stack<>();
    }

    @Nested
    @DisplayName("when new")
    class WhenNew {
        @Test
        @DisplayName("is empty")
        void isEmpty() {
            assertTrue(stack.isEmpty());
        }

        @Test
        @DisplayName("throws EmptyStackException when popped")
        void throwsExceptionWhenPopped() {
            assertThrows(EmptyStackException.class, () -> stack.pop());
        }
    }
}
```

## Parameterized Tests
Нэг тестийг олон өөр параметрээр тестлэх.
```Java{.line-numbers}
@ParameterizedTest
@ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
void palindromes(String candidate) {
    assertTrue(isPalindrome(candidate));
}
```
