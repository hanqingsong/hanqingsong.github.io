---
title: SpringMVC 接收参数的几种方式
categories:
  - Spring
tags:
  - SpringMVC
  - Spring
  - 后端
date: 2018-03-28 10:42:05
---

## 注解

```
A、处理requet uri 部分（这里指uri template中variable，不含queryString部分）的注解： @PathVariable;
B、处理request header部分的注解： @RequestHeader, @CookieValue;
C、处理request body部分的注解：@RequestParam,  @RequestBody;
D、处理attribute类型是注解： @SessionAttributes, @ModelAttribute;
```

1. 接收Request uri中参数: @PathVariable 

```
 @RequestMapping("/books/{bookId}")  
  public void findPet(@PathVariable String userId, @PathVariable String bookId) {      
  }

```

2. 接收request header部分的注解: @RequestHeader,@CookieValue;

```
@RequestMapping("/displayHeaderInfo.do")  
public void displayHeaderInfo(@RequestHeader("Accept-Encoding") String encoding,  
                              @RequestHeader("Keep-Alive") long keepAlive)  {  
  
  
}

@RequestMapping("/displayHeaderInfo.do")  
public void displayHeaderInfo(@CookieValue("JSESSIONID") String cookie)  {  
  
}  
```

3. 接收request body部分的注解：@RequestParam,@RequestBody;
@RequestParam使用场景：
A)常用来处理简单类型的绑定，通过Reqeust.getParameter()获取的String可以直接转化为简单类型的情况；因为使用request.getParameter()方式获取参数，所以可以处理get方式中queryString的值，也可以获取Post方式中body data的值；
B)用来处理Content-Type:为application/x-www-form-urlencoded编码的内容，提交方式GET、POST；
C)该注解有两个属性：value、required;value用来指定要传入值得id名称，required用来指示参数是否必须绑定；

@RequestBody作用：
该注解常用来处理Content-Type:不是 application/x-www-form-urlencoded编码的内容，例如application/json,application/xml等； 它是通过HandlerAdapter配置的HttpMessageConverters来解析post data body，然后绑定到相应的bean上。 因为配置有FormHttpMessageConverter,所以也可以用来处理Application/x-www-form-urlencoded的内容，处理完的结果放在一个MultiValueMap<String,String>里，这种情况特殊需求下使用，详情查看FormHttpMessageConverter api;

4. 接收attribute类型的注解：@SessionAttributes,@ModelAttribute;
@SessionAttributes:
该注解用来绑定HttpSession中的attribute对象的值，便于在方法中的参数中使用。
该注解有value、types两个属性，可以通过名字和类型指定要使用的attribute对象；
```
@Controller  
@RequestMapping("/editPet.do")  
@SessionAttributes("pet")  
public class EditPetForm {  
    // ...  
}  

```

@ModelAttribute
该注解有两个用法：通常用来处理@RequestMapping之前，为请求绑定需要从后台查询的model；
用于参数上时：用来通过名称对应，将相应名称的值绑定到注解的参数bean上；要绑定的值来源于：
A)@SessionAttribute启用的attribute对象上；
B)@ModelAttribute用于方法上时指定的model对象；
C)上述两种情况都没有时，new一个需要绑定的bean对象，然后将request中按名称对应的方式把值绑定到bean中
```
@ModelAttribute  
public Account addAccount(@RequestParam String number) {  
    return accountManager.findAccount(number);  
}  
```
这种方式实际的效果就是在调用@RequestMapping的方法之前，为request对象的model里put("account",Account);

用在参数上的@ModelAttribute示例代码：
```
@RequestMapping(value="/owners/{ownerId}/pets/{petId}/edit", method = RequestMethod.POST)  
public String processSubmit(@ModelAttribute Pet pet) {  
     
}
```

首先查询@SessionAttributes有无绑定的Pet对象，若没有则查询@ModelAttribute方法层面上是否绑定了Pet对象，若没有则URI template中的值按对应的名称绑定到Pet对象的各属性上。


## form表单提交
1. 直接参数名接收
直接通过参数名接收，添加注解@RequestParam, 不添加注解时，参数名要和表单name一致，否则接收不到。

2. 实体bean接收
bean的属性要和表单name一致，可以不添加任何注解，也可以添加注解 @ModelAttribute,括号里的别名可以任意取，也可以不填 

3. 相同参数名可以用数组接收
```
    @RequestMapping(value="/array/form/post/or/get",method={RequestMethod.POST,RequestMethod.GET})  
    public void formPost(String[] userName,String testParam){  
        logger.info("form表单提交，用数组接收相同参数名");  
        logger.info("userName:"+Arrays.toString(userName));  
        logger.info("testParam:"+testParam);  
    }  
```

## 链接请求 
1. 使用@PathVariable注解
2. 使用@RequestParam注解
这里和表单get请求一样 
3. 实体bean接收 
同表单提交一样

## ajax请求
1. ajax请求,参数为json字符串,get请求
用参数名获取json字符串，然后后台对json字符串做处理`JSON.parseObject(json,class)`

2. ajax请求,参数为json字符串,post请求
`必须添加`@RequestBody注解，利用spring框架将json串转成java bean,属性名称要和json字符串一致

3. ajax请求,post请求 json字符串直接用参数名获取
注解@RequestBody，然后用String接收到整个json字符串

4. ajax发送数组格式的字符串
数组格式的字符串，需要手动转换成数组对象
```
    /** 
     * ajax发送数组格式的字符串， 
     * 实际是数组格式的字符串，需要手动转换成数组对象 
     * @param params 
     * @return 
     */  
    @RequestMapping(value="/ajax/post/arr",method=RequestMethod.POST)  
    public String ajaxPostArr(@RequestBody String params){  
        logger.info("ajax传递数组格式的字符串");  
        logger.info("params:"+params);  
        String[] arr = JSON.parseObject(params, String[].class);  
        logger.info(Arrays.toString(arr));  
        return "success";  
    }  
```

5. ajax直接传递数组对象，同时还可以传递其他参数
```

    /** 
     * ajax直接传递数组对象，同时还可以传递其他参数， 
     * 类似表单提交，注意要设置： 
     * traditional：true 
     * contentType:默认 
     *  
     * @param params 
     * @return 
     */  
    @RequestMapping(value="/ajax/post/arr2",method={RequestMethod.POST,RequestMethod.GET})  
    public String ajaxPostArr2(String[] params,String name){  
        logger.info("ajax传递数组对象");  
        logger.info(Arrays.toString(params));  
        logger.info("name:"+name);  
        return "success";  
    }
```

## 后端POST方法接收参数
### 实体bean接收，不添加任何注解,地址参数
 1. ajax 请求,Content type'application/x-www-form-urlencoded;charset=UTF-8',POST

### 实体bean接收，不添加任何注解,body传参 ContentType:x-www-form-urlencoded 
 1. ajax 请求,Content type'application/x-www-form-urlencoded;charset=UTF-8',POST

### 实体bean接收，使用@RequestBody注解
 1. ajax 请求,Content type'application/json;charset=UTF-8',POST

### 实体bean接收，使用@RequestBody注解
1. ajax 请求,Content type'application/json;charset=UTF-8',POST, json.stringify



## 参考资料
https://www.jianshu.com/p/b4b2c38d31ee
https://docs.spring.io/spring/docs/4.3.14.RELEASE/spring-framework-reference/htmlsingle/
https://blog.csdn.net/MyNoteBlog/article/details/72519295

