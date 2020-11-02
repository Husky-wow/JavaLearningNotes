# Swagger

## 一、前言

相信无论是前端还是后端开发，都或多或少地被接口文档折磨过。前端经常抱怨后端给的接口文档与实际情况不一致。后端又觉得编写及维护接口文档会耗费不少精力，经常来不及更新。其实无论是前端调用后端，还是后端调用后端，都期望有一个好的接口文档。但是这个接口文档对于程序员来说，就跟注释一样，经常会抱怨别人写的代码没有写注释，然而自己写起代码起来，最讨厌的，也是写注释。所以仅仅只通过强制来规范大家是不够的，随着时间推移，版本迭代，接口文档往往很容易就跟不上代码了。



## 二、Swagger定义

Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。



## 三、快速搭建

### 1、添加依赖

```xml
<!-- swagger -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

### 2、Swagger配置类（可省略）

```java
package com.woniu.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket createRestApi(){
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.any()).build();
    }

    private ApiInfo apiInfo(){
        return new ApiInfoBuilder()
                .title("Panda API Doc")
                .description("This is a restful api document of Panda.")
                .version("1.0")
                .build();
    }

}

```

- *@ EnableSwagger2*支持Swagger 2的SpringFox支持。
- *DocumentationType.SWAGGER_2*告诉Docket bean我们正在使用Swagger规范的版本2。
- *select（）*创建一个构建器，用于定义哪些控制器及其生成的文档中应包含哪些方法。
- *apis（）*定义要包含的类（控制器和模型类）。这里我们包括所有这些，但您可以通过基础包，类注释等来限制它们。
- *paths（）*允许您根据路径映射定义应包含哪个控制器的方法。我们现在包括所有这些，但您可以使用正则表达式等限制它。

### 3、给功能添加描述信息

```java
package com.woniu.controller;

import com.woniu.pojo.User;
import com.woniu.service.UserService;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.validation.FieldError;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import javax.annotation.Resource;
import javax.servlet.http.HttpSession;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Controller
@Api("用户控制类")
public class UserController {
    @Resource
    private UserService userService;
    @ApiOperation("登录功能")
    @PostMapping("login")
    @ResponseBody
    public Map<String,Object> login(@ApiParam(value = "获得登录用户名和密码",required = true)  User user){
        Map<String,Object> map = new HashMap<>();
        return map;
    }
}

```

## 四、常用注解

 @Api()用于类； 
表示标识这个类是swagger的资源 
\- @ApiOperation()用于方法； 
表示一个http请求的操作 
\- @ApiParam()用于方法，参数，字段说明； 
表示对参数的添加元数据（说明或是否必填等） 
\- @ApiModel()用于类 
表示对类进行说明，用于参数用实体类接收 
\- @ApiModelProperty()用于方法，字段 
表示对model属性的说明或者数据操作更改 
\- @ApiIgnore()用于类，方法，方法参数 
表示这个方法或者类被忽略 
\- @ApiImplicitParam() 用于方法 
表示单独的请求参数 
\- @ApiImplicitParams() 用于方法，包含多个 @ApiImplicitParam