# Fiddler 抓包教程（小白版）

## 一、下载安装

### 1. 下载地址
https://telerik-fiddler.s3.amazonaws.com/fiddler/FiddlerSetup.exe

点击 **Download for Windows**，下载免费版

### 2. 安装
双击安装包 → 一路点 Next → 完成

---

## 二、首次配置（重要！）

打开 Fiddler 后，按以下步骤配置：

### 1. 开启HTTPS抓包
1. 打开 Fiddler
2. **Tools** → **Options** → **HTTPS** → 勾选解密 → **OK**
3. 右侧 **Filters** → 勾选 **Use Filters** → 输入 `*.baidu.com`
4. **Ctrl + X** 清空列表
5. 浏览器打开 www.baidu.com
6. 搜索"测试"
```
菜单栏：Tools → Options → HTTPS

勾选以下两项：
☑ Capture HTTPS CONNECTs
☑ Decrypt HTTPS traffic

点击 OK
```

### 2. 安装证书

弹出提示框，点击 **Yes** 安装证书

如果没弹出，手动安装：
```
Tools → Options → HTTPS → Actions → Trust Root Certificate → Yes
```

### 3. 重启 Fiddler

关闭 Fiddler，重新打开

---

## 四、开始抓包

### 1. 确认抓包已开启

左下角显示 **Capturing** = 正在抓包

如果显示空白，按 **F12** 开启抓包

### 2. 打开浏览器操作

1. 打开浏览器（Chrome/Edge）
2. 访问你要抓包的网站
3. 进行操作（登录、搜索、点击按钮等）
4. Fiddler 左侧列表会显示所有请求

### 3. 查看请求详情

点击左侧某个请求，右侧显示详情：

| 标签 | 内容 |
|------|------|
| **Headers** | 请求头、响应头 |
| **WebForms** | POST表单参数 |
| **JSON** | JSON格式的请求体 |
| **Raw** | 原始报文（完整内容） |

---

## 五、筛选请求（重要！）

请求太多看不过来？用筛选！

### 方法1：按域名筛选

右侧面板 → **Filters** 标签 → 勾选 **Use Filters**

```
Hosts 下拉选择：Show only the following Hosts
输入框填入：api.xxx.com（你要抓的域名）
点击 Actions → Run Filterset now
```

### 方法2：快速筛选

左下角命令行输入：
```
=api          # 只显示URL包含"api"的请求
=login        # 只显示URL包含"login"的请求
@json         # 只显示JSON响应
```

### 方法3：按类型筛选

工具栏下方有快捷按钮：
- 点击 **Hide Images** 隐藏图片
- 点击 **Hide CONNECTs** 隐藏连接请求

---

## 六、查看接口详细信息

### 1. 查看请求URL和方法

点击请求 → 右侧 **Inspectors** → **Headers**

```
Request Headers:
GET https://api.xxx.com/user/info HTTP/1.1
     ↑方法  ↑完整URL
```

### 2. 查看请求参数

**GET请求**：参数在URL里
```
https://api.xxx.com/search?keyword=test&page=1
                           ↑参数
```

**POST请求**：点击 **WebForms** 或 **JSON** 查看请求体

### 3. 查看响应内容

点击请求 → 右侧下半部分 → **JSON** 或 **Raw**

---

## 七、导出接口信息

### 方法1：复制单个请求

右键请求 → **Copy** → **Just URL** 或 **Headers Only**

### 方法2：保存完整请求

右键请求 → **Save** → **Request** → **Entire Request**

### 方法3：导出多个请求

选中多个请求（Ctrl+点击）→ **File** → **Export Sessions** → **Selected Sessions**

---

## 八、整理成接口文档

抓到接口后，按这个格式记录：

```markdown
## 接口名称：用户登录

- 请求地址：https://api.xxx.com/user/login
- 请求方法：POST
- Content-Type：application/json

### 请求参数
| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| username | String | 是 | 用户名 |
| password | String | 是 | 密码 |

### 请求示例
{
  "username": "admin",
  "password": "123456"
}

### 响应示例
{
  "code": 200,
  "data": {
    "token": "xxx",
    "userId": 10001
  }
}
```

---

## 九、抓手机APP接口

### 1. 查看电脑IP

```
Win + R → 输入 cmd → 回车
输入：ipconfig
找到 IPv4 地址，如：192.168.1.100
```

### 2. Fiddler设置

```
Tools → Options → Connections
☑ Allow remote computers to connect
端口：8888（默认）
点击 OK
```

**重启 Fiddler**

### 3. 手机设置代理

```
手机 → 设置 → WiFi → 点击当前连接的WiFi
→ 代理 → 手动

服务器：192.168.1.100（电脑IP）
端口：8888
保存
```

### 4. 手机安装证书

手机浏览器访问：
```
http://192.168.1.100:8888
```

点击页面上的 **FiddlerRoot certificate** 下载证书

安装证书（不同手机方式不同，一般在设置→安全里）

### 5. 开始抓包

操作手机APP，Fiddler就能抓到请求了

---

## 十、常见问题

### Q1：抓不到HTTPS请求？

重新配置证书：
```
Tools → Options → HTTPS → Actions → Reset All Certificates
然后重新勾选 Decrypt HTTPS traffic
重启 Fiddler
```

### Q2：请求列表是空的？

- 按 F12 开启抓包
- 检查左下角是否显示 Capturing

### Q3：手机连不上代理？

- 确保手机和电脑连同一个WiFi
- 关闭电脑防火墙试试
- 重启 Fiddler

### Q4：抓到的内容是乱码？

点击工具栏的 **Decode** 按钮解码

---

## 十一、实战练习

### 练习：抓百度搜索接口

1. 打开 Fiddler
2. 打开浏览器，访问 www.baidu.com
3. 搜索框输入"JMeter"，回车
4. 回到 Fiddler，找到包含 `/s?` 的请求
5. 点击查看请求参数（wd=JMeter）
6. 查看响应内容

恭喜！你已经学会抓包了！

---

## 十二、快捷键

| 快捷键 | 功能 |
|--------|------|
| F12 | 开启/关闭抓包 |
| Ctrl+X | 清空请求列表 |
| Ctrl+F | 搜索 |
| R | 重放请求 |
| Delete | 删除选中请求 |
