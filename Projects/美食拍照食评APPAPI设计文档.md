# 美食拍照食评APPAPI设计文档

## 🔗 **API概述**

### 基础信息
- **API版本**：v1
- **基础URL**：https://api.foodreviewapp.com/v1
- **请求格式**：JSON
- **响应格式**：JSON
- **字符编码**：UTF-8

### 数据传输
- **数据传输方式**：HTTP/HTTPS
- **默认字符编码**：UTF-8
- **压缩方式**：Gzip（可选）

### 响应格式
```json
{
  "code": 0,
  "msg": "success",
  "data": {
    // 返回数据
  },
  "timestamp": 1773069053632
}
```

### 错误响应格式
```json
{
  "code": 401,
  "msg": "Unauthorized",
  "error": {
    "message": "需要登录",
    "code": 1001
  },
  "timestamp": 1773069053632
}
```

## 🔐 **认证与授权**

### 用户认证
- **方式**：JWT（JSON Web Token）
- **获取Token**：通过登录接口获取
- **Token过期**：1小时后过期，需要刷新Token

### 请求头
```
Authorization: Bearer <Token>
Content-Type: application/json
```

### 刷新Token
- **接口**：`POST /auth/refresh`
- **描述**：使用刷新Token获取新的访问Token

## 👤 **用户API**

### 用户注册
- **接口**：`POST /auth/register`
- **描述**：用户注册
- **请求参数**：
  ```json
  {
    "username": "string",
    "email": "string",
    "password": "string",
    "phone": "string"
  }
  ```
- **响应参数**：
  ```json
  {
    "user": {
      "id": "string",
      "username": "string",
      "email": "string",
      "avatar_url": "string"
    },
    "token": "string",
    "refresh_token": "string"
  }
  ```

### 用户登录
- **接口**：`POST /auth/login`
- **描述**：用户登录
- **请求参数**：
  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```
- **响应参数**：
  ```json
  {
    "user": {
      "id": "string",
      "username": "string",
      "email": "string",
      "avatar_url": "string"
    },
    "token": "string",
    "refresh_token": "string"
  }
  ```

### 获取用户信息
- **接口**：`GET /user/info`
- **描述**：获取当前登录用户的信息
- **请求参数**：无
- **响应参数**：
  ```json
  {
    "id": "string",
    "username": "string",
    "email": "string",
    "avatar_url": "string",
    "phone": "string",
    "bio": "string"
  }
  ```

### 更新用户信息
- **接口**：`PUT /user/info`
- **描述**：更新用户信息
- **请求参数**：
  ```json
  {
    "username": "string",
    "avatar_url": "string",
    "phone": "string",
    "bio": "string"
  }
  ```
- **响应参数**：
  ```json
  {
    "id": "string",
    "username": "string",
    "email": "string",
    "avatar_url": "string",
    "phone": "string",
    "bio": "string"
  }
  ```

## 🍽️ **食评API**

### 创建食评
- **接口**：`POST /reviews`
- **描述**：创建新的食评
- **请求参数**：
  ```json
  {
    "shop_id": "string",
    "content": "string",
    "rating": 5,
    "tags": ["string"],
    "images": ["string"]
  }
  ```
- **响应参数**：
  ```json
  {
    "id": "string",
    "user_id": "string",
    "shop_id": "string",
    "content": "string",
    "rating": 5,
    "tags": ["string"],
    "images": ["string"],
    "created_at": "2026-03-09T15:06:34Z",
    "updated_at": "2026-03-09T15:06:34Z"
  }
  ```

### 获取食评列表
- **接口**：`GET /reviews`
- **描述**：获取用户的食评列表
- **请求参数**：
  - page: 页码（默认1）
  - limit: 每页数量（默认10）
  - shop_id: 店铺ID（可选）
  - created_at: 创建时间（可选）
- **响应参数**：
  ```json
  {
    "total": 100,
    "page": 1,
    "limit": 10,
    "data": [
      {
        "id": "string",
        "user_id": "string",
        "shop_id": "string",
        "content": "string",
        "rating": 5,
        "tags": ["string"],
        "images": ["string"],
        "created_at": "2026-03-09T15:06:34Z",
        "updated_at": "2026-03-09T15:06:34Z"
      }
    ]
  }
  ```

### 获取单个食评
- **接口**：`GET /reviews/:id`
- **描述**：获取单个食评的详细信息
- **请求参数**：
  - id: 食评ID
- **响应参数**：
  ```json
  {
    "id": "string",
    "user_id": "string",
    "shop_id": "string",
    "content": "string",
    "rating": 5,
    "tags": ["string"],
    "images": ["string"],
    "created_at": "2026-03-09T15:06:34Z",
    "updated_at": "2026-03-09T15:06:34Z"
  }
  ```

### 更新食评
- **接口**：`PUT /reviews/:id`
- **描述**：更新食评
- **请求参数**：
  ```json
  {
    "content": "string",
    "rating": 5,
    "tags": ["string"],
    "images": ["string"]
  }
  ```
- **响应参数**：
  ```json
  {
    "id": "string",
    "user_id": "string",
    "shop_id": "string",
    "content": "string",
    "rating": 5,
    "tags": ["string"],
    "images": ["string"],
    "created_at": "2026-03-09T15:06:34Z",
    "updated_at": "2026-03-09T15:06:34Z"
  }
  ```

### 删除食评
- **接口**：`DELETE /reviews/:id`
- **描述**：删除食评
- **请求参数**：
  - id: 食评ID
- **响应参数**：
  ```json
  {
    "code": 0,
    "msg": "success"
  }
  ```

## 📷 **图片识别API**

### 菜品识别
- **接口**：`POST /image/detect`
- **描述**：识别图片中的菜品
- **请求参数**：
  ```json
  {
    "image_url": "string" // 图片URL或base64编码
  }
  ```
- **响应参数**：
  ```json
  {
    "dish_name": "string",
    "confidence": 0.95,
    "ingredients": ["string"],
    "calories": 500,
    "nutrition_info": {
      "protein": 20,
      "fat": 10,
      "carbohydrates": 60
    }
  }
  ```

### OCR文字识别
- **接口**：`POST /image/ocr`
- **描述**：识别图片中的文字
- **请求参数**：
  ```json
  {
    "image_url": "string" // 图片URL或base64编码
  }
  ```
- **响应参数**：
  ```json
  {
    "text": "string"
  }
  ```

## 🤖 **AI食评生成API**

### 生成食评
- **接口**：`POST /ai/generate-review`
- **描述**：基于菜品信息生成食评
- **请求参数**：
  ```json
  {
    "dish_name": "string",
    "content": "string",
    "style": "professional", // 可选：professional, humorous, concise
    "shop_info": {
      "name": "string",
      "address": "string",
      "category": "string"
    }
  }
  ```
- **响应参数**：
  ```json
  {
    "content": "string",
    "tags": ["string"],
    "images": ["string"]
  }
  ```

## 🏪 **店铺API**

### 搜索店铺
- **接口**：`GET /shops/search`
- **描述**：搜索店铺
- **请求参数**：
  - keyword: 关键词
  - lat: 纬度（可选，用于地理位置搜索）
  - lng: 经度（可选，用于地理位置搜索）
  - radius: 搜索半径（可选，默认1000米）
- **响应参数**：
  ```json
  {
    "total": 100,
    "page": 1,
    "limit": 10,
    "data": [
      {
        "id": "string",
        "name": "string",
        "address": "string",
        "latitude": 39.9042,
        "longitude": 116.4074,
        "category": "string",
        "rating": 4.5,
        "average_price": 100
      }
    ]
  }
  ```

### 获取店铺信息
- **接口**：`GET /shops/:id`
- **描述**：获取店铺详细信息
- **请求参数**：
  - id: 店铺ID
- **响应参数**：
  ```json
  {
    "id": "string",
    "name": "string",
    "address": "string",
    "latitude": 39.9042,
    "longitude": 116.4074,
    "category": "string",
    "rating": 4.5,
    "average_price": 100,
    "phone": "string",
    "business_hours": "string"
  }
  ```

## 📱 **社交分享API**

### 分享食评
- **接口**：`POST /shares`
- **描述**：分享食评到社交平台
- **请求参数**：
  ```json
  {
    "review_id": "string",
    "platform": "xiaohongshu", // 可选：xiaohongshu, weibo, wechat, instagram
    "content": "string",
    "images": ["string"]
  }
  ```
- **响应参数**：
  ```json
  {
    "id": "string",
    "review_id": "string",
    "platform": "xiaohongshu",
    "platform_post_id": "string",
    "content": "string",
    "images": ["string"],
    "created_at": "2026-03-09T15:06:34Z",
    "updated_at": "2026-03-09T15:06:34Z"
  }
  ```

### 获取分享统计
- **接口**：`GET /shares/stats`
- **描述**：获取分享统计信息
- **请求参数**：
  - review_id: 食评ID（可选）
  - platform: 平台（可选）
- **响应参数**：
  ```json
  {
    "total_shares": 10,
    "platform_stats": {
      "xiaohongshu": 5,
      "weibo": 3,
      "wechat": 2
    },
    "total_likes": 100,
    "total_comments": 20
  }
  ```

## 🔍 **搜索API**

### 搜索食评
- **接口**：`GET /search/reviews`
- **描述**：搜索食评
- **请求参数**：
  - keyword: 关键词
  - shop_id: 店铺ID（可选）
  - rating: 评分（可选）
- **响应参数**：
  ```json
  {
    "total": 100,
    "page": 1,
    "limit": 10,
    "data": [
      {
        "id": "string",
        "user_id": "string",
        "shop_id": "string",
        "content": "string",
        "rating": 5,
        "tags": ["string"],
        "images": ["string"],
        "created_at": "2026-03-09T15:06:34Z"
      }
    ]
  }
  ```

## 📈 **统计API**

### 获取用户统计信息
- **接口**：`GET /stats/user`
- **描述**：获取用户的统计信息
- **请求参数**：
  - user_id: 用户ID（可选，默认当前登录用户）
- **响应参数**：
  ```json
  {
    "total_reviews": 10,
    "total_shares": 5,
    "total_likes": 100,
    "favorite_cuisine": ["string"],
    "average_rating": 4.5
  }
  ```

### 获取系统统计信息
- **接口**：`GET /stats/system`
- **描述**：获取系统的统计信息
- **请求参数**：无
- **响应参数**：
  ```json
  {
    "total_users": 1000,
    "total_reviews": 5000,
    "total_shares": 10000,
    "active_users": 500
  }
  ```

## 🎯 **错误码**

### 通用错误码
- 0: 成功
- 400: 请求参数错误
- 401: 未授权
- 403: 禁止访问
- 404: 资源未找到
- 500: 服务器内部错误

### 业务错误码
- 1001: 需要登录
- 1002: 用户不存在
- 1003: 密码错误
- 1004: 食评不存在
- 1005: 店铺不存在
- 1006: 图片识别失败
- 1007: 食评生成失败

## 🔒 **安全考虑**

### API签名
- 使用JWT Token进行身份认证
- 对敏感接口使用HTTPS传输

### 接口限流
- 对API请求进行限流
- 对异常请求进行阻断

### 数据验证
- 对输入数据进行严格验证
- 防止SQL注入和XSS攻击

## 📱 **移动应用集成**

### 应用配置
- 提供配置文件和初始化代码
- 支持多种开发框架（React Native, Flutter）

### SDK集成
- 提供iOS和Android SDK
- 提供详细的集成文档和示例代码

## 🚀 **部署建议**

### 服务器架构
- 使用Node.js+Express或Python+Django
- 使用MongoDB和Redis
- 使用Nginx作为反向代理

### 监控与日志
- 使用APM工具进行性能监控
- 记录详细的API请求和响应日志
- 定期进行安全扫描和漏洞修复

## 🎯 **扩展性考虑**

### 接口版本管理
- 使用API版本控制
- 提供详细的API变更记录

### 功能扩展
- 预留接口扩展空间
- 支持插件和模块扩展

## 📝 **结论**

本API设计文档提供了美食拍照食评APP的完整接口规范，为前端和后端开发提供了清晰的指导。设计考虑了性能、安全性和可扩展性，为后续开发和维护奠定了基础。
