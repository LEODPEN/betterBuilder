# BetterBuilder

[![GitHub license](https://img.shields.io/github/license/LEODPEN/BetterBuilder)](https://github.com/LEODPEN/BetterBuilder/blob/main/LICENSE) 
![Build status](https://img.shields.io/badge/build-passing-brightgreen)
[![Version](https://img.shields.io/badge/version-1.0.2-orange)](https://github.com/LEODPEN/BetterBuilder/releases)
---
BetterBuilder is a [Java annotation processor](https://docs.oracle.com/javase/8/docs/api/javax/annotation/processing/Processor.html) used for
automatically generating better builder codes([builder design pattern](https://en.wikipedia.org/wiki/Builder_pattern#Java)) with fluent get/set methods, 
which can make coding much more comfortable.

## Getting BetterBuilder

> BetterBuilder doesn't add any runtime dependencies to your code.

### Directly reach the jar

Download from [releases](https://github.com/LEODPEN/betterBuilder/releases).
(Just add it to your classpath)
### Maven

BetterBuilder(v1.0.2) has already been published to Central https://repo1.maven.org/maven2/.

Example Maven settings:

```xml
<dependency>
  <groupId>cn.mpy634</groupId>
  <artifactId>BetterBuilder</artifactId>
  <version>1.0.2</version>
  <scope>provided</scope>
</dependency>
```

## Usage

> Simple example;[See how to customize](#customization)
 
Given a class "Student":

```java
import cn.mpy634.annotation.BetterBuilder;
// All configurations are default.
@BetterBuilder
public class Student {
    private String name;
    private Integer ID;
}
```
The compiled code could be :
```java
public class Student {
    private String name;
    private Integer ID;
    public Student ID(Integer ID) {this.ID = ID;return this;}
    public Integer ID() {return this.ID;}
    public Student name(String name) {this.name = name;return this;}
    public String name() {return this.name;}
    public static Student.StudentBuilder builder() {return new Student.StudentBuilder();}
    public Student(String name, Integer ID) {this.name = name;this.ID = ID;}
    public Student() {}
    public static class StudentBuilder {
        private String name;
        private Integer ID;
        private StudentBuilder() {}
        public Student.StudentBuilder name(String name) {this.name = name;return this;}
        public Student.StudentBuilder ID(Integer ID) {this.ID = ID;return this;}
        public Student build() {return new Student(this.name, this.ID);}
    }
}
```
Therefore you can code like `Student stu = Student.builder().ID(xx).name(xx)....build().ID(xx).name(xx)...` .

You can also customize the generated code.
 
## Customization

### FluentSet switch

Once make {fluentSet = false}, BetterBuilder will not generate set methods.
```java
@BetterBuilder(fluentSet = false)
public class Student {
    ...
}
```

### FluentGet switch

Once make {fluentGet = false}, BetterBuilder will not generate get methods.
```java
@BetterBuilder(fluentGet = false)
public class Student {
    ...
}
```

### Set type

Make {setType = 0 / 1} to change the return type of generated set methods.

Given a field `private Integer ID;`, 2 kinds of set methods are available.

When setType = 0, which is default(strongly suggested):
```java
@BetterBuilder(setType = 0)
public class Student {
    private Integer ID;
    public Student ID(Integer ID){this.ID = ID; return this;}
}
```
when setType = 1, set methods will return nothing:
```java
@BetterBuilder(setType = 1)
public class Student {
    private Integer ID;
    public void ID(Integer ID){this.ID = ID;}
}
```

### NoBuilder switch

Once make {noBuilder = true}, BetterBuilder will not generate builder methods (nor the allArgsConstructor).
```java
@BetterBuilder(noBuilder = true)
public class Student {
    ...
}
```

### Field ignore

Make any fields annotated with {@IgnoreGet or @IgnoreSet}, BetterBuilder will
not generate the get or set method for it.
```java
@BetterBuilder
public class Student {
    @IgnoreSet
    private String 牛;
    @IgnoreGet
    private Integer 年;
    @IgnoreGet
    @IgnoreSet
    private Student 大;
    private List<Boolean> 吉;
    
}
```
It is for those **fields that aren't allowed to be changed or accessed after 
initialization**.


## Todo list

- [x] fluent - builder / test
    - [x] noBuilder
- [x] fluent - set / test
    - [x] chain set options
    - [x] ignore set
- [x] fluent - get / test
    - [x] ignore get
- [x] compatible with lombok
- [ ] type-safe builder / @Required for certain fields 

...

## Others

+ Any bugs or suggestions? Plz make PRs or [issues](https://github.com/LEODPEN/BetterBuilder/issues).
