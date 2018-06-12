---
title: 错误处理
date: 2018-04-19 17:32:54
categories: spring
tags: 
    - spring
    - error
---

## ExceptionHandler
使用ExceptionHandler这个注解就能统一处理某个异常，避免重复处理代码
使用ControllerAdivce可以为所有控制器处理异常
```
@ControlerAdvice
public class AppWideExceptionHandler {
    @ExceptionHandler(DuplicateSpittleException.class)
    public String duplicateSpittleHandler(){
        return "error/duplicate";
    }
}
```
