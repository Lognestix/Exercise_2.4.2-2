# В pom.xml добавлены поддержка junit-jupiter и подходящий ему maven-surefire-plugin, в свойства добавлена кодировка для работы с Maven без IDEA

```xml
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.2</version>
            </plugin>
        </plugins>
    </build>
```

# Код Java находящийся в этом репозитории

```Java
package ru.netology.bonus;

public class BonusService {
    public long calculate(long amount, boolean registered) {
        int percent = registered ? 3 : 1;
        long bonus = amount * percent / 100 / 100;
        long limit = 500;
        if (bonus > limit) {
            bonus = limit;
        }
        return bonus;
    }
}
```
```Java
package ru.netology.bonus;

import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvFileSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class BonusServiceTest {
    @ParameterizedTest
    //Задействована аннотация @CsvFileSource, с файлом, содержащим данные для автотестов, data.csv:
    @CsvFileSource(resources = "/data.csv")
    void shouldCalculate(String test, long amount, boolean registered, long expected) {
        BonusService service = new BonusService();

        // вызываем целевой метод:
        long actual = service.calculate(amount, registered);

        // производим проверку (сравниваем ожидаемый и фактический):
        assertEquals(expected, actual);
    }
}
```

# Данные файла data.csv

```csv
"'Registered user, bonus under limit'", "100060", "true", "30"
"'Registered user, bonus over limit'", "100000060", "true", "500"
"'Unregistered user, bonus under limit'", "100060", "false", "10"
"'Unregistered user, bonus over limit'", "100000060", "false", "500"
```