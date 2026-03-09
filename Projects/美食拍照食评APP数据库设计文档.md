# 美食拍照食评APP数据库设计文档

## 📊 **数据库概述**

### 数据库类型
- **主要数据库**：MongoDB（文档数据库，用于存储非结构化数据）
- **缓存数据库**：Redis（用于高速数据检索和缓存）
- **关系型数据库**：MySQL（可选，用于存储结构化数据）

### 设计原则
- 数据冗余最小化
- 查询效率最大化
- 可扩展性和可维护性
- 安全性和数据完整性

## 📝 **用户相关表**

### 用户表 (users)
| 字段名 | 类型 | 长度 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|------|
| _id | ObjectId | - | 用户唯一标识 | 是 | 主键 |
| username | String | 50 | 用户名 | 是 | 唯一 |
| email | String | 100 | 邮箱地址 | 是 | 唯一 |
| password | String | 255 | 密码（加密存储） | 是 | - |
| avatar_url | String | 255 | 头像URL | 否 | - |
| phone | String | 20 | 手机号码 | 否 | - |
| bio | String | 500 | 个人简介 | 否 | - |
| created_at | Date | - | 创建时间 | 是 | - |
| updated_at | Date | - | 更新时间 | 是 | - |
| last_login_at | Date | - | 最后登录时间 | 否 | - |

### 用户偏好设置表 (user_preferences)
| 字段名 | 类型 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|
| _id | ObjectId | 记录唯一标识 | 是 | 主键 |
| user_id | ObjectId | 用户ID | 是 | 索引 |
| favorite_cuisine | Array | 喜欢的菜系 | 否 | - |
| dietary_restrictions | Array | 饮食限制（如素食、 gluten-free） | 否 | - |
| spice_preference | Number | 辣度偏好（1-5级） | 否 | - |
| notifications_enabled | Boolean | 是否启用通知 | 是 | - |
| email_verified | Boolean | 邮箱是否验证 | 是 | - |
| created_at | Date | 创建时间 | 是 | - |
| updated_at | Date | 更新时间 | 是 | - |

## 🍽️ **食评相关表**

### 食评表 (reviews)
| 字段名 | 类型 | 长度 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|------|
| _id | ObjectId | - | 食评唯一标识 | 是 | 主键 |
| user_id | ObjectId | - | 用户ID | 是 | 索引 |
| shop_id | ObjectId | - | 店铺ID | 是 | 索引 |
| content | String | 2000 | 食评内容 | 是 | - |
| rating | Number | - | 评分（1-5星） | 是 | 索引 |
| tags | Array | - | 标签（如“辣”、“性价比高”） | 否 | - |
| images | Array | - | 图片URL数组 | 否 | - |
| created_at | Date | - | 创建时间 | 是 | 索引 |
| updated_at | Date | - | 更新时间 | 是 | - |
| published_platforms | Array | - | 已发布的平台 | 否 | - |
| is_public | Boolean | - | 是否公开 | 是 | 索引 |

### 菜品识别结果表 (dish_identifications)
| 字段名 | 类型 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|
| _id | ObjectId | - | 记录唯一标识 | 是 | 主键 |
| review_id | ObjectId | - | 食评ID | 是 | 索引 |
| dish_name | String | 100 | 菜品名称 | 是 | - |
| confidence | Number | - | 识别置信度（0-1） | 是 | - |
| ingredients | Array | - | 菜品成分 | 否 | - |
| calories | Number | - | 热量（卡路里） | 否 | - |
| nutrition_info | Object | - | 营养信息（蛋白质、脂肪、碳水化合物） | 否 | - |
| image_url | String | 255 | 菜品图片URL | 是 | - |
| created_at | Date | - | 创建时间 | 是 | - |

## 🏪 **店铺相关表**

### 店铺表 (shops)
| 字段名 | 类型 | 长度 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|------|
| _id | ObjectId | - | 店铺唯一标识 | 是 | 主键 |
| name | String | 100 | 店铺名称 | 是 | 索引 |
| address | String | 255 | 店铺地址 | 是 | - |
| latitude | Number | - | 纬度 | 否 | 地理空间索引 |
| longitude | Number | - | 经度 | 否 | 地理空间索引 |
| category | String | 50 | 店铺类别（如“中餐”、“西餐”） | 否 | 索引 |
| average_price | Number | - | 人均消费 | 否 | - |
| rating | Number | - | 评分（1-5星） | 否 | 索引 |
| phone | String | 20 | 联系电话 | 否 | - |
| business_hours | String | 100 | 营业时间 | 否 | - |
| shop_info | Object | - | 其他信息（如菜系、推荐菜） | 否 | - |
| created_at | Date | - | 创建时间 | 是 | - |
| updated_at | Date | - | 更新时间 | 是 | - |

### 店铺评分表 (shop_ratings)
| 字段名 | 类型 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|
| _id | ObjectId | - | 记录唯一标识 | 是 | 主键 |
| shop_id | ObjectId | - | 店铺ID | 是 | 索引 |
| user_id | ObjectId | - | 用户ID | 是 | 索引 |
| overall_rating | Number | - | 整体评分 | 是 | - |
| service_rating | Number | - | 服务评分 | 否 | - |
| food_rating | Number | - | 食物评分 | 否 | - |
| environment_rating | Number | - | 环境评分 | 否 | - |
| value_rating | Number | - | 性价比评分 | 否 | - |
| created_at | Date | - | 创建时间 | 是 | - |
| updated_at | Date | - | 更新时间 | 是 | - |

## 📱 **社交分享相关表**

### 分享记录表 (shares)
| 字段名 | 类型 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|
| _id | ObjectId | - | 记录唯一标识 | 是 | 主键 |
| review_id | ObjectId | - | 食评ID | 是 | 索引 |
| platform | String | 50 | 分享平台（如“小红书”、“微博”） | 是 | 索引 |
| platform_post_id | String | 100 | 平台帖子ID | 否 | - |
| share_count | Number | - | 分享次数 | 是 | - |
| like_count | Number | - | 点赞数 | 否 | - |
| comment_count | Number | - | 评论数 | 否 | - |
| read_count | Number | - | 阅读数 | 否 | - |
| created_at | Date | - | 创建时间 | 是 | 索引 |
| updated_at | Date | - | 更新时间 | 是 | - |

## 📊 **缓存和统计**

### 用户统计信息表 (user_statistics)
| 字段名 | 类型 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|
| _id | ObjectId | - | 记录唯一标识 | 是 | 主键 |
| user_id | ObjectId | - | 用户ID | 是 | 索引 |
| total_reviews | Number | - | 食评总数 | 是 | - |
| total_shares | Number | - | 分享总数 | 是 | - |
| total_likes | Number | - | 总点赞数 | 是 | - |
| favorite_cuisine | Array | - | 喜欢的菜系统计 | 否 | - |
| most_visited_category | String | - | 最常访问的类别 | 否 | - |
| average_rating | Number | - | 平均评分 | 否 | - |
| last_active_date | Date | - | 最后活跃时间 | 是 | - |
| created_at | Date | - | 创建时间 | 是 | - |
| updated_at | Date | - | 更新时间 | 是 | - |

### 系统统计信息表 (system_statistics)
| 字段名 | 类型 | 说明 | 必填 | 索引 |
|--------|------|------|------|------|
| _id | ObjectId | - | 记录唯一标识 | 是 | 主键 |
| stat_date | Date | - | 统计日期 | 是 | 索引 |
| new_users | Number | - | 新增用户数 | 是 | - |
| total_reviews | Number | - | 总食评数 | 是 | - |
| total_shares | Number | - | 总分享数 | 是 | - |
| active_users | Number | - | 活跃用户数 | 是 | - |
| avg_reviews_per_user | Number | - | 平均食评数 | 是 | - |
| created_at | Date | - | 创建时间 | 是 | - |
| updated_at | Date | - | 更新时间 | 是 | - |

## 🔗 **关系图**

### 用户与食评关系
- 一个用户可以有多个食评（1对多）
- 一个食评只能属于一个用户（多对1）

### 食评与菜品识别关系
- 一个食评可以有多个菜品识别结果（1对多）
- 一个菜品识别结果只能属于一个食评（多对1）

### 食评与分享记录关系
- 一个食评可以有多个分享记录（1对多）
- 一个分享记录只能属于一个食评（多对1）

### 用户与店铺关系
- 一个用户可以访问多个店铺（1对多）
- 一个店铺可以被多个用户访问（多对1）

## 🔍 **查询优化**

### 常用查询优化
- 对用户查询食评时，使用user_id和created_at索引
- 对搜索店铺时，使用category和rating索引
- 对地理查询时，使用经纬度字段的地理空间索引
- 对分享统计查询时，使用platform和created_at索引

### 缓存策略
- 使用Redis缓存热门食评和店铺信息
- 缓存用户会话和认证信息
- 对频繁访问的数据进行预热

## 🔒 **数据安全**

### 加密字段
- 用户密码使用bcrypt加密存储
- 敏感信息如手机号码和邮箱部分脱敏

### 访问控制
- 严格的用户权限验证
- 数据访问日志和审计机制

### 备份与恢复
- 定期自动备份数据库
- 灾难恢复计划和测试

## 📈 **数据增长与维护**

### 分区策略
- 按时间分区用户表和食评表
- 对历史数据进行归档

### 索引维护
- 定期优化查询和索引
- 删除不再使用的索引

### 数据迁移
- 使用数据迁移工具（如MongoDB Compass）
- 确保数据一致性和完整性

## 🎯 **扩展性考虑**

### 读写分离
- 实现读写分离，提高查询效率
- 使用MongoDB的副本集功能

### 分片策略
- 对大数据集进行分片处理
- 按用户ID或时间范围进行分片

### 云服务
- 使用云数据库服务（如MongoDB Atlas）
- 自动扩展和负载均衡

## 📝 **结论**

本数据库设计文档提供了美食拍照食评APP的完整数据库架构，旨在支持应用的核心功能，如用户管理、食评记录、菜品识别和社交分享。设计考虑了数据完整性、查询效率和可扩展性，为后续开发和维护奠定了坚实基础。
