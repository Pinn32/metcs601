---
date: 2026-04-26
tags:
  - nextjs
  - webdev
  - web
  - webapp
  - frontend
  - cs
---

# lec-14 Material UI

**四个 MUI Libs: MUI, Joy-UI, Base-UI, MUI-System**  

**MUI:**  
Material Design styles, 可以用 **==Emotion== 或 ==styled-components== 来 override** 已有的 styles  
**Joy-UI:**   
非 Material Design styles, ==**mobile-first**==  
**Base-UI:**   
无 style, 纯组件, ==**mobile-first**==, 需要用 **`useMemo()`** 记住组件和 style 来避免 re-render  
**MUI System:**   
可以自定义 style, 通过设置 **`sx` prop** 来自定义 style




---

# lec-15 MongoDB

**Non-Relational, Schema-Less** Database  
Data stored as ==**BSON (Binary JSON)**==  
MongoDB: cluster -> database -> **collection** -> ==**document**== (ie. each entry)  
如果没有 collection / 没有 doc id, ==**MongoDB 会自动帮你创建 collection / 创建 doc id**==  

==**MongoDB CRUD**==  
**Create:** `insertOne()` ; `insertMany()`  
**Read:** `find()`  
**Update:**  
`updateOne()` ; `updateMany()` ;  
`replaceOne()` ;  
`findOneAndUpdate()` ; `findOneAndReplace()` ;  
`findAndModify()` ;  
`bulkWrite()`  
**Delete:** `deleteOne()` ; `deleteMany()`  



---

# lec-16 GraphQL & Apollo
**OOP Design Patterns to ==prevent SQL injection==:**  
==**DAO:**== Data Access Object  
==**ORM:**== Object Relational Mapping  

**Apollo Hooks for CRUD in GraphQL:**    
**Create, Update, Delete:** `useMutation()`  
**Read:** `useQuery()`  

---

# lec-17 Server-Side Architecture

**Tier-1:** Everything in One Place, 无延迟, 超安全, 只有本地 server, 没有 network/scalability, 难以维护和更新  
**Tier-2:** Client & Server+DB (server 和 db 互相依赖, coupled)  
**Tier-3:** Three-Part System: Client -> Server -> Database  (互不依赖: fully decoupled)
**N-Tier:** 比以上三种更复杂, scalability 最强, 互不依赖 (fully decoupled), 但复杂.  

**N-Tier:**  
**传输优化:** 由于 request ==**messages**== 太多太复杂且贵, 需要使用 ==**Loading Balancing**== 算法来优化 message request 步骤.  
**传输途径: Client, server, database, and combination** can communicate through **messages** method.  
**Caching:** 可以在任何地方 (client, server, db, combination)  
**Caching:** Browser / Server-Side / Database / CDN (Content Delivery Networks) Caching


---

# lec-18 Cyber Security

**CI/CD:** Continuous Integration, Delivery, and ==Deployment==

**Prevent SQL injection: ==DAO, ORM==** (OOP design patterns)

**Federated Authentication:** ==SSO, OAuth==  
**==Identity Provider:==** 第三方登录平台, eg. 谷歌, GitHub

**CSRF: Cross-Site Request Forgery**  

**OAuth:**  
**scope:** 从第三方平台获取用户的哪些功能/信息 (eg. read email / send email / repo info / username...)  
**==state:==** 用户登录时随机生成的状态id, 防止别人跨 tab 拦截你的密码, aka ==防止CSRF==  
**response_type=code:** 临时通行证, 代替 token, 避免暴露 token 并防止 CSRF  

---

# lec-19 React Native

**创建 Expo App:** `npx create-expo-app@latest`  

**React Native 特殊文件名:**  
`_layout.tsx` -- static components (RootLayout)  
`index.tsx` -- conditional rendered components (类 page.tsx)  

**全部没有单位:** ==unitless== in all dimensions, `width: 300; height: 600;`   

**只有 ReactNative 可用的 hook:** `useDeviceOrientation()`   

---

# lec-20 Cookie, Session, CORS

```tsx
// HTTP-Only cookie, 不可被 JS 读取
httpOnly: true,

// Secure Cookie: 只允许加密网络通过 https 通信, 不允许非加密通讯
secure: true, 

// 默认 "lax" (松), 改成 "strict" 防止 CSRF
sameSite: "lax" / "strict" / "none",

// Persistence Cookie: 7天 (单位ms) 后过期, 存在 Disk 里
// Session Cookie: 不设置 maxAge, 关闭即过期, 存在 RAM 里
maxAge: 7*24*60*60*1000,
```

| 类型 | 存储位置 | 容量 | 生命周期 | 是否跨 tab | 随 HTTP 发给服务器 | 安全性 | 适用场合 |
|------|----------|------|----------|------------|------------------------|------------------|------|
| Cookie | Browser cookie storage | ~4KB–5KB | 可设置 | ✔ | ✔ | ✔ | 记住登录状态 |
| Session Storage | RAM | ~5MB | 关 tab 即清除 | ❌ | ❌ | ❌ | 临时用 |
| Local Storage | Disk | ~5–10MB | 永久 (需手动清除) | ✔ | ❌ | ⚠️ | 持久本地偏好设置 |

**CORS: Cross-Origin Resource Sharing**
