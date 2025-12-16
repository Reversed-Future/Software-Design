# API接口规范文档

## 1. 项目概述

本项目是一个旅游景点平台，提供景点查询、帖子分享、商品购买等功能。API设计遵循RESTful风格，支持多角色用户（管理员、商家、游客）。

### 1.1 基础URL
```
/api/v1
```

### 1.2 响应格式
所有API接口返回统一格式：

```typescript
interface ApiResponse<T> {
  success: boolean;       // 请求是否成功
  data?: T;               // 响应数据
  message?: string;       // 错误或成功消息
}
```

## 2. 认证服务

### 2.1 用户登录
- **路径**：`/auth/login`
- **方法**：`POST`
- **功能**：用户登录
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | email | string | 是 | 用户邮箱 |
  | password | string | 是 | 用户密码 |

- **响应数据**：
  ```typescript
  ApiResponse<User> {
    success: boolean;
    data: User; // 包含token
    message?: string;
  }
  ```

### 2.2 用户注册
- **路径**：`/auth/register`
- **方法**：`POST`
- **功能**：用户注册
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | userData | Partial<User> | 是 | 用户基本信息 |
  | password | string | 是 | 用户密码 |

- **响应数据**：
  ```typescript
  ApiResponse<User> {
    success: boolean;
    data: User; // 包含token
    message?: string;
  }
  ```

### 2.3 获取待审核商家
- **路径**：`/admin/pending-merchants`
- **方法**：`GET`
- **功能**：获取所有待审核的商家用户
- **权限**：管理员

- **响应数据**：
  ```typescript
  ApiResponse<User[]> {
    success: boolean;
    data: User[];
    message?: string;
  }
  ```

### 2.4 更新用户状态
- **路径**：`/admin/users/:userId/status`
- **方法**：`PUT`
- **功能**：更新用户状态（审核商家）
- **权限**：管理员
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | status | 'active' \| 'rejected' | 是 | 用户状态 |

- **响应数据**：
  ```typescript
  ApiResponse<boolean> {
    success: boolean;
    data: boolean;
    message?: string;
  }
  ```

## 3. 景点服务

### 3.1 获取景点列表
- **路径**：`/attractions`
- **方法**：`GET`
- **功能**：获取景点列表，支持多条件过滤
- **请求参数**（查询参数）：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | province | string | 否 | 省份 |
  | city | string | 否 | 城市 |
  | county | string | 否 | 区县 |
  | query | string | 否 | 搜索关键词 |
  | tag | string | 否 | 标签筛选 |

- **响应数据**：
  ```typescript
  ApiResponse<Attraction[]> {
    success: boolean;
    data: Attraction[];
    message?: string;
  }
  ```

### 3.2 获取景点详情
- **路径**：`/attractions/:id`
- **方法**：`GET`
- **功能**：获取单个景点详情

- **响应数据**：
  ```typescript
  ApiResponse<Attraction> {
    success: boolean;
    data: Attraction;
    message?: string;
  }
  ```

### 3.3 创建景点
- **路径**：`/attractions`
- **方法**：`POST`
- **功能**：创建新景点
- **权限**：管理员
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | title | string | 是 | 景点名称 |
  | description | string | 是 | 景点描述 |
  | address | string | 是 | 景点地址 |
  | province | string | 是 | 省份 |
  | city | string | 是 | 城市 |
  | county | string | 是 | 区县 |
  | tags | string[] | 否 | 标签列表 |
  | imageUrl | string | 否 | 封面图片URL |
  | gallery | string[] | 否 | 相册图片URL列表 |
  | openHours | string | 否 | 开放时间 |
  | drivingTips | string | 否 | 驾车提示 |

- **响应数据**：
  ```typescript
  ApiResponse<Attraction> {
    success: boolean;
    data: Attraction;
    message?: string;
  }
  ```

### 3.4 更新景点
- **路径**：`/attractions/:id`
- **方法**：`PUT`
- **功能**：更新景点信息
- **权限**：管理员
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | title | string | 否 | 景点名称 |
  | description | string | 否 | 景点描述 |
  | address | string | 否 | 景点地址 |
  | province | string | 否 | 省份 |
  | city | string | 否 | 城市 |
  | county | string | 否 | 区县 |
  | tags | string[] | 否 | 标签列表 |
  | imageUrl | string | 否 | 封面图片URL |
  | gallery | string[] | 否 | 相册图片URL列表 |
  | openHours | string | 否 | 开放时间 |
  | drivingTips | string | 否 | 驾车提示 |

- **响应数据**：
  ```typescript
  ApiResponse<Attraction> {
    success: boolean;
    data: Attraction;
    message?: string;
  }
  ```

### 3.5 删除景点
- **路径**：`/attractions/:id`
- **方法**：`DELETE`
- **功能**：删除景点
- **权限**：管理员

- **响应数据**：
  ```typescript
  ApiResponse<boolean> {
    success: boolean;
    data: boolean;
    message?: string;
  }
  ```

## 4. 帖子服务

### 4.1 获取帖子列表
- **路径**：`/posts`
- **方法**：`GET`
- **功能**：获取帖子列表
- **请求参数**（查询参数）：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | attractionId | string | 否 | 景点ID |

- **响应数据**：
  ```typescript
  ApiResponse<Post[]> {
    success: boolean;
    data: Post[]; // 仅返回active状态的帖子
    message?: string;
  }
  ```

### 4.2 创建帖子
- **路径**：`/posts`
- **方法**：`POST`
- **功能**：创建新帖子
- **权限**：登录用户
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | attractionId | string | 是 | 关联景点ID |
  | userId | string | 是 | 用户ID |
  | username | string | 是 | 用户名 |
  | content | string | 是 | 帖子内容 |
  | rating | number | 否 | 1-5星级评分 |
  | imageUrl | string | 否 | 图片URL |

- **响应数据**：
  ```typescript
  ApiResponse<Post> {
    success: boolean;
    data: Post;
    message?: string;
  }
  ```

### 4.3 举报帖子
- **路径**：`/posts/:id/report`
- **方法**：`POST`
- **功能**：举报帖子
- **权限**：登录用户

- **响应数据**：
  ```typescript
  ApiResponse<boolean> {
    success: boolean;
    data: boolean;
    message?: string;
  }
  ```

## 5. 商品服务

### 5.1 获取商品列表
- **路径**：`/products`
- **方法**：`GET`
- **功能**：获取商品列表
- **请求参数**（查询参数）：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | merchantId | string | 否 | 商家ID |
  | attractionId | string | 否 | 关联景点ID |

- **响应数据**：
  ```typescript
  ApiResponse<Product[]> {
    success: boolean;
    data: Product[];
    message?: string;
  }
  ```

### 5.2 获取商品详情
- **路径**：`/products/:id`
- **方法**：`GET`
- **功能**：获取单个商品详情

- **响应数据**：
  ```typescript
  ApiResponse<Product> {
    success: boolean;
    data: Product;
    message?: string;
  }
  ```

### 5.3 创建商品
- **路径**：`/products`
- **方法**：`POST`
- **功能**：创建新商品
- **权限**：商家用户
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | merchantId | string | 是 | 商家ID |
  | merchantName | string | 否 | 商家名称 |
  | attractionId | string | 否 | 关联景点ID |
  | name | string | 是 | 商品名称 |
  | description | string | 是 | 商品描述 |
  | price | number | 是 | 商品价格 |
  | stock | number | 是 | 库存数量 |
  | imageUrl | string | 否 | 商品图片URL |

- **响应数据**：
  ```typescript
  ApiResponse<Product> {
    success: boolean;
    data: Product; // 自动包含attractionName（如果提供了attractionId）
    message?: string;
  }
  ```

## 6. 订单服务

### 6.1 创建订单
- **路径**：`/orders`
- **方法**：`POST`
- **功能**：创建新订单
- **权限**：登录用户
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | userId | string | 是 | 用户ID |
  | items | CartItem[] | 是 | 购物车商品列表 |
  | total | number | 是 | 订单总金额 |

- **响应数据**：
  ```typescript
  ApiResponse<Order> {
    success: boolean;
    data: Order;
    message?: string;
  }
  ```

### 6.2 获取订单列表
- **路径**：`/orders`
- **方法**：`GET`
- **功能**：获取订单列表
- **权限**：登录用户
- **请求参数**（查询参数）：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | userId | string | 否 | 用户ID |
  | merchantId | string | 否 | 商家ID |

- **响应数据**：
  ```typescript
  ApiResponse<Order[]> {
    success: boolean;
    data: Order[];
    message?: string;
  }
  ```

### 6.3 更新订单状态
- **路径**：`/orders/:id/status`
- **方法**：`PUT`
- **功能**：更新订单状态
- **权限**：商家用户或管理员
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | status | Order['status'] | 是 | 订单状态 |
  | trackingNumber | string | 否 | 物流单号 |

- **响应数据**：
  ```typescript
  ApiResponse<Order> {
    success: boolean;
    data: Order;
    message?: string;
  }
  ```

## 7. 管理员服务

### 7.1 获取举报内容
- **路径**：`/admin/reported-content`
- **方法**：`GET`
- **功能**：获取所有被举报的帖子
- **权限**：管理员

- **响应数据**：
  ```typescript
  ApiResponse<Post[]> {
    success: boolean;
    data: Post[]; // 仅返回reported状态的帖子
    message?: string;
  }
  ```

### 7.2 审核内容
- **路径**：`/admin/content/:id/moderate`
- **方法**：`POST`
- **功能**：审核被举报的帖子
- **权限**：管理员
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | action | 'approve' \| 'delete' | 是 | 审核操作 |

- **响应数据**：
  ```typescript
  ApiResponse<boolean> {
    success: boolean;
    data: boolean;
    message?: string;
  }
  ```

### 7.3 文件上传
- **路径**：`/upload`
- **方法**：`POST`
- **功能**：上传文件
- **权限**：登录用户
- **请求参数**：
  | 参数名 | 类型 | 必填 | 描述 |
  |--------|------|------|------|
  | file | File | 是 | 上传的文件 |

- **响应数据**：
  
  ```string
  文件的Base64编码字符串
  ```



## 8. 用户信息修改

### 8.1 用户信息获取

## 9. 数据类型定义

### 9.1 User 类型
```typescript
interface User {
  id: string;
  username: string;
  email: string;
  role: UserRole;
  token?: string;
  status: 'active' | 'pending' | 'rejected';
  qualificationUrl?: string; // 商家资质证书URL
}
```

### 9.2 UserRole 枚举
```typescript
enum UserRole {
  GUEST = 'guest',
  TRAVELER = 'traveler',
  MERCHANT = 'merchant',
  ADMIN = 'admin'
}
```

### 9.3 Attraction 类型
```typescript
interface Attraction {
  id: string;
  title: string;
  description: string;
  address: string;
  region: string; // 格式化显示字符串
  province?: string;
  city?: string;
  county?: string;
  tags: string[];
  imageUrl: string;
  gallery?: string[]; // 附加照片
  openHours?: string;
  drivingTips?: string;
}
```

### 9.4 Post 类型
```typescript
interface Post {
  id: string;
  attractionId: string;
  userId: string;
  username: string;
  content: string;
  rating?: number; // 1-5星评分
  imageUrl?: string;
  likes: number;
  comments: Comment[];
  createdAt: string;
  status: 'active' | 'reported' | 'hidden';
}
```

### 9.5 Comment 类型
```typescript
interface Comment {
  id: string;
  userId: string;
  username: string;
  content: string;
  createdAt: string;
}
```

### 9.6 Product 类型
```typescript
interface Product {
  id: string;
  merchantId: string;
  merchantName: string;
  attractionId?: string; // 关联景点
  attractionName?: string; // 关联景点名称（用于显示）
  name: string;
  description: string;
  price: number;
  stock: number;
  imageUrl: string;
}
```

### 9.7 CartItem 类型
```typescript
interface CartItem extends Product {
  quantity: number;
}
```

### 9.8 Order 类型
```typescript
interface Order {
  id: string;
  userId: string;
  items: CartItem[];
  total: number;
  status: 'pending' | 'shipped' | 'delivered' | 'cancelled';
  trackingNumber?: string;
  createdAt: string;
}
```

## 10. 错误码说明

| 错误码 | 描述 |
|--------|------|
| 400 | 请求参数错误 |
| 401 | 未授权访问 |
| 403 | 权限不足 |
| 404 | 资源不存在 |
| 500 | 服务器内部错误 |
