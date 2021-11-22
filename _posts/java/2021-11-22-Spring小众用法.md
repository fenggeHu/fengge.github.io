记录一些用得不多且有用的

# Spring RestController 统一处理返回值
1，继承ResponseBodyAdvice实现beforeBodyWrite方法，加上注解@RestControllerAdvice(basePackages = {"hu.jinfeng.xxx"})
    basePackages限制范围，可排除一些不必要的转换
举例：
```java
@Slf4j
@RestControllerAdvice(basePackages = {"hu.jinfeng.controller"})
public class AdminResponseBodyAdvice implements ResponseBodyAdvice<Object> {

    @Override
    public boolean supports(MethodParameter returnType, Class<? extends HttpMessageConverter<?>> converterType) {
        return true;
    }

    @Override
    public Object beforeBodyWrite(Object body, MethodParameter methodParameter, MediaType mediaType, Class<? extends HttpMessageConverter<?>> aClass, ServerHttpRequest serverHttpRequest, ServerHttpResponse serverHttpResponse) {
        if (body instanceof ResultModel) {
            // 忽略
        } else if (body instanceof BizException) {
            BizException be = (BizException) body;
            body = ResultModel.error(be.getCode(), be.getReason());
        } else if (body instanceof Throwable) {
            body = ResultModel.error(ErrorCode.INTERNAL_SERVER_ERROR);
        } else {
            body = ResultModel.ok(body);
        }

        return body;
    }
}
```
2，实现全局异常的拦截处理，也是使用注解@RestControllerAdvice 。
举例：
```java
@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {

    /**
     * 处理业务异常
     */
    @ExceptionHandler(BizException.class)
    public BizException handleBizException(HttpServletRequest request, BizException ex) {
        return ex;
    }

    /**
     * 处理一些未知异常
     */
    @ExceptionHandler(Throwable.class)
    public BizException handleThrowable(HttpServletRequest request, Throwable ex) {
        log.error(request.getPathInfo(), ex);
        return new BizException(ErrorCode.INTERNAL_SERVER_ERROR, ex.getMessage());
    }
}
```