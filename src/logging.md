# **Logging**

@Slf4J annotation can be used for logging purpose.

example of logging something →

```java
log.error("error: "+errorMessage);
log.info("This is an example of logging");
```

To Store the logs in a separate file we need to have a xml file and the name of the xml file needs to be logback.xml

Here is a template for logback.xml file.

To Store the logs in a separate file we need to have an xml file and the name of the xml file needs to be logback.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <property name="LOGS" value="./logs"/>

    <appender name="Console"
              class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>
                %black(%d{ISO8601, Asia/Dhaka}) ${hostName} [%blue(%thread)] %highlight(%-5level) %yellow(%logger{16}) - line = %green(%line): %msg%n%throwable
            </Pattern>
        </layout>
    </appender>

    <appender name="RollingFile"
              class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>../${LOGS}/agronomix-livestock/agronomix-livestock.log</file>
        <encoder
                class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <charset>UTF-8</charset>
            <Pattern>%d{ISO8601, Asia/Dhaka} ${hostName} %thread %-5level %logger{16} - line = %line: %msg%n%throwable
            </Pattern>
        </encoder>

        <rollingPolicy
                class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- rollover daily and when the file reaches 3 MegaBytes -->
            <fileNamePattern>../${LOGS}/agronomix-livestock/archived/agronomix-livestock-%d{yyyy-MM-dd, Asia/Dhaka}.%i.log
            </fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy
                    class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>3MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
    </appender>

    <!-- LOG everything at INFO level -->
    <springProfile name="dev">
        <root level="info">
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="Console"/>
        </root>

        <logger name="com.ds.agronomix.livestock" level="debug" additivity="false">
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="Console"/>
        </logger>
    </springProfile>

    <springProfile name="test">
        <root level="info">
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="Console"/>
        </root>
        <logger name="com.ds.agronomix.livestock" level="debug" additivity="false">
            <appender-ref ref="RollingFile"/>
        </logger>
    </springProfile>

    <springProfile name="live">
        <root level="info">
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="Console"/>
        </root>
        <logger name="com.ds.agronomix.livestock" level="debug" additivity="false">
            <appender-ref ref="RollingFile"/>
        </logger>
    </springProfile>

    <springProfile name="swagger">
        <root level="info">
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="Console"/>
        </root>
        <logger name="com.ds.agronomix.livestock" level="debug" additivity="false">
            <appender-ref ref="RollingFile"/>
        </logger>
    </springProfile>

</configuration>
```

All you need to do is to change the name of the files and stuffs.
