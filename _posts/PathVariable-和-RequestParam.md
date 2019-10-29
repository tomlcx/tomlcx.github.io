---
title: '@PathVariable 和 @RequestParam'
date: 2018-08-05 22:59:46
tags: springboot
---
@pathVariable和RequestParam的区别：顾名思义，前者是从路径中获取变量，也就是把路径当做变量，后者是从请求里面获取参数
从你的请求来看： 

/Springmvc/user/page.do?pageSize=3&pageNow=2 

pageSize和pageNow应该是属于参数而不是路径，所以应该添加@RequestParam的注解。 

如果做成如下URL，则可以使用@PathVariable 
 /SSSP/emp/7
 someUrl/{paramId}
这里的paramId是路径中的变量，应使用@pathVariable


1、 @PathVariable 
当使用@RequestMapping URI template 样式映射时， 即 someUrl/{paramId}, 这时的paramId可通过 @Pathvariable注解绑定它传过来的值到方法的参数上。

@RequestParam 

A） 常用来处理简单类型的绑定，通过Request.getParameter() 获取的String可直接转换为简单类型的情况（ String--> 简单类型的转换操作由ConversionService配置的转换器来完成）；因为使用request.getParameter()方式获取参数，所以可以处理get 方式中queryString的值，也可以处理post方式中 body data的值；

B）用来处理Content-Type: 为 application/x-www-form-urlencoded编码的内容，提交方式GET、POST；

C) 该注解有两个属性： value、required； value用来指定要传入值的id名称，required用来指示参数是否必须绑定；