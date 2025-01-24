# Java

https://en.wikipedia.org/wiki/Java_version_history

|       Version       |  Type   | Major Version |    Release date     |
| :-----------------: | :-----: | :-----------: | :-----------------: |
|       JDK 1.0       |         |      45       |  23rd January 1996  |
|       JDK 1.1       |         |      45       | 18th February 1997  |
|      J2SE 1.2       |         |      46       |  4th December 1998  |
|      J2SE 1.3       |         |      47       |    8th May 2000     |
|      J2SE 1.4       |         |      48       | 13th February 2002  |
|   J2SE 5.0 (1.5)    |         |      49       | 30th September 2004 |
|   Java SE 6 (1.6)   |         |      50       | 11th December 2006  |
|   Java SE 7 (1.7)   |         |      51       |   28th July 2011    |
| **Java SE 8 (1.8)** | **LTS** |      52       |   18th March 2014   |
|   Java SE 9 (1.9)   |         |      53       | 21st September 2017 |
|  Java SE 10 (1.10)  |         |      54       |   20th March 2018   |
|   **Java SE 11**    | **LTS** |      55       | 25th September 2018 |
|     Java SE 12      |         |      56       |   19th March 2019   |
|     Java SE 13      |         |      57       | 17th September 2019 |
|     Java SE 14      |         |      58       |   17th March 2020   |
|     Java SE 15      |         |      59       | 16th September 2020 |
|     Java SE 16      |         |      60       |   16th March 2021   |
|   **Java SE 17**    | **LTS** |      61       | 14th September 2021 |
|     Java SE 18      |         |      62       |   22nd March 2022   |
|     Java SE 19      |         |      63       | 20th September 2022 |
|     Java SE 20      |         |      64       |   21st March 2023   |
|   **Java SE 21**    | **LTS** |      65       | 19th September 2023 |
|     Java SE 22      |         |      66       |   19th March 2024   |
|     Java SE 23      |         |      67       | 17th September 2024 |
|     Java SE 24      |         |      68       |     March 2025      |
|   **Java SE 25**    | **LTS** |      69       |   September 2025    |

# Java/Jakarta EE

https://en.wikipedia.org/wiki/Jakarta_EE

| Platform version |          Release           |   Java SE Support    | Important Changes                                            |
| :--------------: | :------------------------: | :------------------: | :----------------------------------------------------------- |
|  Jakarta EE 11   | Planned for June/July 2024 |      Java SE 21      | Data                                                         |
|  Jakarta EE 10   |         2022-09-22         | Java SE 17, Java SE 11 | Removal of deprecated items in Servlet, Faces, CDI and EJB (Entity Beans and Embeddable Container). CDI-Build Time. |
|  Jakarta EE 9.1  |         2021-05-25         | Java SE 11, Java SE 8 | JDK 11 support                                               |
|   Jakarta EE 9   |         2020-12-08         |      Java SE 8       | API namespace move from `javax` to `jakarta`                 |
|   Jakarta EE 8   |         2019-09-10         |      Java SE 8       | Full compatibility with Java EE 8                            |
|    Java EE 8     |         2017-08-31         |      Java SE 8       | HTTP/2 and CDI based Security                                |
|    Java EE 7     |         2013-05-28         |      Java SE 7       | WebSocket, JSON and HTML5 support                            |
|    Java EE 6     |         2009-12-10         |      Java SE 6       | CDI managed Beans and REST                                   |
|    Java EE 5     |         2006-05-11         |      Java SE 5       | Java annotations and Generics in Java                        |
|     J2EE 1.4     |         2003-11-11         |       J2SE 1.4       | WS-I interoperable web services                              |
|     J2EE 1.3     |         2001-09-24         |       J2SE 1.3       | Java connector architecture                                  |
|     J2EE 1.2     |         1999-12-17         |       J2SE 1.2       | Initial specification release                                |

# Servlet

https://en.wikipedia.org/wiki/Jakarta_Servlet

|  Servlet API version  |    Released    |       Platform       |                      Important Changes                       |
| :-------------------: | :------------: | :------------------: | :----------------------------------------------------------: |
|  Jakarta Servlet 6.0  |  May 31, 2022  |    Jakarta EE 10     | remove deprecated features and implement requested enhancements |
|  Jakarta Servlet 5.0  |  Oct 9, 2020   |     Jakarta EE 9     | API moved from package `javax.servlet` to `jakarta.servlet`  |
| Jakarta Servlet 4.0.3 |  Sep 10, 2019  |     Jakarta EE 8     |                Renamed from "Java" trademark                 |
|   Java Servlet 4.0    |    Sep 2017    |      Java EE 8       |                            HTTP/2                            |
|   Java Servlet 3.1    |    May 2013    |      Java EE 7       | Non-blocking I/O, HTTP protocol upgrade mechanism (WebSocket) |
|   Java Servlet 3.0    | December 2009  | Java EE 6, Java SE 6 | Pluggability, Ease of development, Async Servlet, Security, File Uploading |
|   Java Servlet 2.5    | September 2005 | Java EE 5, Java SE 5 |           Requires Java SE 5, supports annotation            |
|   Java Servlet 2.4    | November 2003  |  J2EE 1.4, J2SE 1.3  |                   web.xml uses XML Schema                    |
|   Java Servlet 2.3    |  August 2001   |  J2EE 1.3, J2SE 1.2  |                     Addition of `Filter`                     |
|   Java Servlet 2.2    |  August 1999   |  J2EE 1.2, J2SE 1.2  | Becomes part of J2EE, introduced independent web applications in .war files |
|   Java Servlet 2.1    | November 1998  |     Unspecified      | First official specification, added `RequestDispatcher`, `ServletContext` |
|   Java Servlet 2.0    | December 1997  |       JDK 1.1        |     Part of April 1998 Java Servlet Development Kit 2.0      |
|   Java Servlet 1.0    | December 1996  |                      |  Part of June 1997 Java Servlet Development Kit (JSDK) 1.0   |

# Spring                                   

https://github.com/spring-projects/spring-framework/wiki/Spring-Framework-Versions

## JDK Version Range

- Spring Framework 7.0.x: JDK 17-27 (expected)
- Spring Framework 6.2.x: JDK 17-25 (expected)
- Spring Framework 6.1.x: JDK 17-23
- Spring Framework 6.0.x: JDK 17-21
- Spring Framework 5.3.x: JDK 8-21 (as of 5.3.26)

## Java/Jakarta EE Versions

- Spring Framework 7.0.x: Jakarta EE 11 (jakarta namespace)
- Spring Framework 6.2.x: Jakarta EE 9-10 (jakarta namespace)
- Spring Framework 6.1.x: Jakarta EE 9-10 (jakarta namespace)
- Spring Framework 6.0.x: Jakarta EE 9-10 (jakarta namespace)
- Spring Framework 5.3.x: Java EE 7-8 (javax namespace) 

# SpringBoot

## 2.X

requires Java 8

## 3.x

requires Java 17 or later

# Tomcat

https://tomcat.apache.org/whichversion.html

| **Servlet Spec** | **JSP Spec** | **EL Spec** | **WebSocket Spec** | **Authentication Spec (JASPIC)** | **Apache Tomcat Version** | **Latest Released Version** | **Supported Java Versions** |
| :--------------- | :----------- | :---------- | :----------------- | :------------------------------- | :------------------------ | :-------------------------- | :-------------------------- |
| 6.1              | 4.0          | 6.0         | 2.2                | 3.1                              | 11.0.x                    | 11.0.2                      | 17 and later                |
| 6.0              | 3.1          | 5.0         | 2.1                | 3.0                              | 10.1.x                    | 10.1.34                     | 11 and later                |
| 4.0              | 2.3          | 3.0         | 1.1                | 1.1                              | 9.0.x                     | 9.0.98                      | 8 and later                 |

# Maven

- 3.8+ : requires JDK 1.7 or above
- 3.9+ : requires JDK 8 or above
- 4.0+ : requires JDK 17 or above

# Logback

https://logback.qos.ch/dependencies.html

## 1.3.X

requires Java 8

## 1.5.X

requires Java 11 or later
