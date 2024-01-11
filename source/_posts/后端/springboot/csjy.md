---
title: SpringBoot参数校验
date: 2023-12-30 16:49:39
category: 后端
tags:
  - SpringBoot
  - SpringBoot项目必备
  - SpringBoot参数校验
---
# SpringBoot参数校验
```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-validation</artifactId>
   <version>{}</version>
</dependency>
```

## @Valid注解
- 由javax提供（标准`JSR-303`规范）
- 使用地方：`方法`、`方法参数`、`成员属性（字段）`
- 功能：【支持嵌套验证】

| 注解                          | 作用                                                                                           | 类型                                                                                                                                                                            |
|-----------------------------|----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `@AssertFalse`              | 限制必须为false                                                                                   | `boolean、Boolean`                                                                                                                                                             |
| `@AssertTrue`               | 限制必须为true                                                                                    | `同上`                                                                                                                                                                          |
| `@Future`                   | 限制必须是一个将来的日期                                                                                 | `Date、Calendar、Instant、LocalDate、LocalDateTime、LocalTime、MonthDay、OffsetDateTime、OffsetTime、Year、YearMonth、ZonedDateTime、HijrahDate、JapaneseDate、MinguoDate、ThaiBuddhistDate` |
| `@FutureOrPresent`          | 未来或当前的日期，此处的present概念是相对于使用约束的类型定义的。例如校验的参数为Year year = Year.now();此时约束是一年，那么“当前”将表示当前的整个年份。 | `同上`                                                                                                                                                                          |
| `@PastOrPresent`            | 过去或者当前时间，和@FutureOrPresent类似                                                                 | `同上`                                                                                                                                                                          |
| `@Past`                     | 限制必须是一个过去的日期                                                                                 | `同上`                                                                                                                                                                          |
| `@Max(value)`               | 限制必须为一个不大于指定值的数字                                                                             | `BigDecimal、BigInteger、byte, short, int, long`                                                                                                                                |
| `@Min(value)`               | 限制必须为一个不小于指定值的数字                                                                             | `同上`                                                                                                                                                                          |
| `@Size(max,min)`            | 限制字符长度必须在min到max之间                                                                           | `同上`                                                                                                                                                                          |
| `@DecimalMax(value)`        | 限制必须为一个不大于指定值的数字                                                                             | `同上`                                                                                                                                                                          |
| `@DecimalMin(value)`        | 限制必须为一个不小于指定值的数字                                                                             | `同上`                                                                                                                                                                          |
| `@Digits(integer,fraction)` | 限制必须为一个小数，且整数部分的位数不能超过integer，小数部分的位数不能超过fraction                                            | `同上`                                                                                                                                                                          |
| `@PositiveOrZero`           | 包含正数和零，空元素有效可以校验通过                                                                           | `同上`                                                                                                                                                                          |
| `@Negative`                 | 绝对的负数，不能包含零，空元素有效可以校验通过                                                                      | `同上`                                                                                                                                                                          |
| `@NegativeOrZero`           | 包含负数和零，空元素有效可以校验通过                                                                           | `同上`                                                                                                                                                                          |
| `@Positive`                 | 绝对的正数，不能包含零，空元素有效可以校验通过                                                                      | `同上`                                                                                                                                                                          |
| `@NotBlank`                 | 验证注解的元素值不为空（不为null、trim()去除首位空格后长度为0）                                                        | `String`                                                                                                                                                                      |
| `@Pattern(value)`           | 限制必须符合指定的正则表达式                                                                               | `同上`                                                                                                                                                                          |
| `@Email`                    | 验证注解的元素值是Email，也可以通过正则表达式和flag指定自定义的email格式                                                  | `同上`                                                                                                                                                                          |
| `@NotEmpty`                 | 验证注解的元素值不为null且不为空（字符串长度不为0、集合大小不为0）                                                         | `CharSequence、Collection、Map、Array`                                                                                                                                           |
| `@NotNull`                  | 限制必须不为null                                                                                   | `所有类型`                                                                                                                                                                        |
| `@Null`                     | 限制只能为null                                                                                    | `同上`                                                                                                                                                                          |
### 使用
- 嵌套校验
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Project {
 
    @NotBlank(message = "ID不能为空")
    private String id;
 
    @NotBlank
    @Pattern(regexp = "[a-zA-Z0-9]", message = "只允许输入数字和字母")
    private String strValue;

    @NotNull(message = "不能为空")
    @Min(value = -99, message = "值不能小于-99")
    @Max(value = 100, message = "值不能超过100")
    private Integer intValue;

    @NotNull(message = "不能为空")
    @Negative(message = "值必须为负数")
    private Integer negativeValue;

    @NotNull(message = "不能为空")
    @Valid// todo 嵌套校验
    private Team team;
 
}

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Team {
 
    @FutureOrPresent(message = "只能输入当前年份或未来的年份")
    private Year nowYear;
 
    @JsonFormat(timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    @Future(message = "只能是未来的时间")
    private Date futureTime;
 
    @JsonFormat(timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    @Past(message = "只能是过去的时间")
    private Date pastTime;
 
    @Email(message = "请输入正确的邮箱")
    private String email;

    @NotNull(message = "不能为空")
    @Valid// todo 嵌套校验
    private List<Member> list;
 
}

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Member {
 
    @NotBlank(message = "姓名不能为空")
    private String name;
 
}

@RestController
@RequestMapping("/valid")
public class TestValidController {

    @PostMapping("/userInfo")
    public BaseResponse userInfo(@Valid @RequestBody Team team) {
        return new BaseResponse(team);
    }
}
```

##  @Validated注解
- 由Spring提供（`Spring's JSR-303`规范）
- 使用地方：`类`、`方法`、`方法参数`
- 功能：【支持分组验证】【能配合`@Valid`进行嵌套验证】

### 使用
- 一、分组校验
  - 场景：保存草稿时某些字段不用校验，提交时都要校验，更新时id要校验不为空；
  - 1、自定义分组
```java
public interface MyValidGroup {
 
    // 保存草稿
    interface SaveDraft { }
    
    // 提交表单
    interface Submit { }
 
    // 更新数据
    interface Update { }
    
}
```
  - 2、设置分组（不设置分组时属于`Default.class`默认分组）
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Project {
 
    @NotBlank(message = "ID不能为空", groups = {MyValidGroup.Update.class})// 更新时要校验id不为空
    private String id;
    
    @NotBlank(message = "项目老大不能为空", groups = {MyValidGroup.SaveDraft.class,MyValidGroup.Submit.class,MyValidGroup.Update.class})
    private String leader;
 
    @NotBlank(message = "ID不能为空")
    @Pattern(regexp = "[a-zA-Z0-9]", message = "只允许输入数字和字母")
    private String strValue;

    @NotNull(message = "不能为空")
    @Min(value = -99, message = "值不能小于-99")
    @Max(value = 100, message = "值不能超过100")
    private Integer intValue;

    @NotNull(message = "不能为空")
    @Negative(message = "值必须为负数")
    private Integer negativeValue;

    @NotNull(message = "不能为空")
    @Valid// todo 嵌套校验
    private Team team;
 
}

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Team {
 
    @FutureOrPresent(message = "只能输入当前年份或未来的年份")
    private Year nowYear;
 
    @JsonFormat(timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    @Future(message = "只能是未来的时间")
    private Date futureTime;
 
    @JsonFormat(timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    @Past(message = "只能是过去的时间")
    private Date pastTime;
 
    @Email(message = "请输入正确的邮箱")
    private String email;

    @NotNull(message = "不能为空")
    @Valid// todo 嵌套校验
    private List<Member> list;
 
}

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Member {
 
    @NotBlank(message = "姓名不能为空")
    private String name;
 
}

@RestController
@RequestMapping("/valid")
public class TestValidController {

    // todo Validated不配置时校验默认分组
    @PostMapping("/all")
    public BaseResponse all(@Validated @RequestBody ProjectDTO testAnnotationDto) {
        return new BaseResponse(testAnnotationDto);
    }
    // todo 保存草稿：校验草稿分组
    @PostMapping("/saveDraft")
    public BaseResponse saveDraft(@Validated(value = {TestValidGroup.SaveDraft.class}) @RequestBody ProjectDTO testAnnotationDto) {
        return new BaseResponse(testAnnotationDto);
    }
    // todo 提交：校验提交分组和默认分组
    @PostMapping("/submit")
    public BaseResponse submit(@Validated(value = {MyValidGroup.Submit.class, Default.class}) @RequestBody ProjectDTO testAnnotationDto) {
        return new BaseResponse(testAnnotationDto);
    }
    // todo 更新：校验更新分组和默认分组
    @PostMapping("/update")
    public BaseResponse update(@Validated(value = {MyValidGroup.Update.class, Default.class}) @RequestBody ProjectDTO testAnnotationDto) {
        return new BaseResponse(testAnnotationDto);
    }
}
```

- 二、分组校验指定顺序`@GroupSequence`
  - 使用在`分组接口`上
```java
public interface MyValidGroup {

  // 先校验XXX分组，再校验YYY分组
  @GroupSequence(value = {XXX.class, YYY.class})
  interface Update { }

  interface XXX { }

  interface YYY { }

}

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class Project {
    @NotBlank(message = "ID不能为空", groups = {MyValidGroup.XXX.class})// 更新时要校验id不为空
    private String id;

    @NotBlank(message = "项目老大不能为空", groups = {MyValidGroup.YYY.class})
    private String leader;
}

@RestController
@RequestMapping("/valid")
public class TestValidController {
  @PostMapping("/post")
  public BaseResponse testValidPostRequest(@Validated(value = {MyValidGroup.Update.class}) @RequestBody ProjectDTO testAnnotationDto) {
    return new BaseResponse(testAnnotationDto);
  }
}
```

- 三、快速失败机制（默认是校验所有参数，返回所有有问题的）
```java
@Configuration
public class ValidConfig {
 
    @Bean
    public Validator validator() {
        ValidatorFactory validatorFactory = Validation.byProvider(HibernateValidator.class).configure()
                .failFast(true)// todo 快速失败模式
                .buildValidatorFactory();
        return validatorFactory.getValidator();
    }
    
}
```

- 四、自定义校验注解（百度）
  - `@Constraint(validatedBy = XXXValidator.class)`：指定我们自定义的校验类
  - `public class XXXValidator implements ConstraintValidator<XXX, String> {}`：自定义校验类

- 五、全局异常处理，统一返回校验异常信息
```java
@Slf4j
@RestControllerAdvice
public class ExceptionHandlerConfig {
    /**
     * 拦截valid参数校验返回的异常，并转化成基本的返回样式
     */
    @ExceptionHandler(value = MethodArgumentNotValidException.class)
    public Result dealMethodArgumentNotValidException(MethodArgumentNotValidException e) {
        log.error("校验异常：", e);
        List<ObjectError> allErrors = e.getBindingResult().getAllErrors();
        String message = allErrors.stream()
                .map(s -> s.getDefaultMessage())
                .collect(Collectors.joining(";"));
        return Result.builder().code("-10").msg(message).build();
    }
}
```

- 六、不同分组下采用不同校验
```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@SuperBuilder
public class Base {
 
    @NotBlank.List({
            @NotBlank(message = "Project不能为空", groups = {MyValidGroup.Project.class}),
            @NotBlank(message = "Team不能为空", groups = {MyValidGroup.Team.class})
    })
    private String baseId;
 
    @Max(value = 10, message = "Project不能大于10", groups = {MyValidGroup.Project.class})
    @Max(value = 30, message = "Team不能大于30", groups = {MyValidGroup.Team.class})
    private Integer number;
}
```

- 

