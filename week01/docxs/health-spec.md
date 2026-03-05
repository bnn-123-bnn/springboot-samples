```
# 系统健康检查与信息接口规格文档

## 1. 核心目标
- 在基础版之上，开发系统健康检查接口及扩展接口，用于监控应用运行状态，并验证多环境配置与全局异常处理。

## 2. 业务规则

### 2.1 必做接口：健康检查
- 接口路径：GET /api/health
- 返回字段：
  - appName：应用名称
  - version：应用版本
  - time：当前服务器时间（yyyy-MM-dd HH:mm:ss）
  - status：运行状态，固定为 "UP"

### 2.2 选做接口1：系统信息
- 接口路径：GET /api/system/info
- 返回字段：
  - appName：应用名称
  - port：运行端口
  - javaVersion：Java 版本
  - osName：操作系统名称

### 2.3 全局异常处理
- 访问不存在的接口时，返回：
  - code：500
  - msg："系统异常，请联系管理员"
  - data：null

## 3. 技术约束
- 使用 Spring Boot 3.x
- 多环境配置：dev(8080), test(8081), prod(8082)
- 统一返回类型：ResultVO<T>
- 全局异常处理：@RestControllerAdvice

## 4. 输入输出

### 4.1 健康检查接口
**请求**：GET /api/health
**响应**：
```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "appName": "springboot-hello",
    "version": "1.0.0",
    "time": "2026-03-05 21:00:00",
    "status": "UP"
  }
}

5. 验收标准
能在 dev/test/prod 三种环境下启动
访问 /api/health 返回包含所有字段的 JSON
访问不存在的接口返回统一 500 错误
```

