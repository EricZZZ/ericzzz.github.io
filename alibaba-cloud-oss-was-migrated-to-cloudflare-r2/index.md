# 阿里云 OSS 迁移到 CloudFlare R2


思路：使用 OSSBrowser 把所有文件下载到本地，再使用 Rclone 工具，把本地文件夹同步到 CloudFlare R2 中。

## OSSBrowser

OSSBrowser 是阿里云官方提供的 OSS 图形化管理工具，提供类似 Windows 资源管理器的功能。使用 OSSBrowser 可以快速完成存储空间（Bucket）和文件（Object）的相关操作

[下载地址](https://oss.console.aliyun.com/services/tools)

### 创建 RAM 用户的 AccessKey，AccessKey Secret

- 登录 [RAM 控制台](https://ram.console.aliyun.com/)。
- 在左侧导航栏，选择**身份管理** > **用户**。
- 在**用户**页面，单击目标 RAM 用户名称。
- 在**用户 AccessKey **区域，单击**创建 AccessKey**。
- 根据界面提示完成安全验证。
- 在**创建 AccessKey **对话框，查看 AccessKey ID 和 AccessKey Secret。
- 您可以单击**下载 CSV 文件**，下载 AccessKey 信息。单击**复制**，复制 AccessKey 信息。
- 单击**确定**。
 
![阿里云 RAM](https://image.ericzzz.com/2023/09/05/6173af98-d151-4706-a6fc-87bfa7663e7c.png)

### 授权

需要给 RAM 用户开通`AliyunOSSFullAccess`，`AliyunRAMFullAccess`，`AliyunSTSAssumeRoleAccess`的权限。

![阿里云 RAM 授权](https://image.ericzzz.com/2023/09/05/979627fc-5aed-4ca6-851c-973514fd3914.png)

### 登录

![登录界面](https://image.ericzzz.com/2023/09/05/ecca0eaf-a0ab-4db7-9075-292844216622.png)

`Endpoint（地域节点）`：选择自定义，不是 Bucket 域名，例如`oss-cn-beijing.aliyuncs.com`。

![Endpoint](https://image.ericzzz.com/2023/09/05/3258d9aa-c32e-4031-9fa1-c036b2b4e198.png)

`AccessKeyId`：填入第一步申请的`AccessKey`。
`AccessKeySecret`：填入第一步申请的`AccessKey Secret`。

### 使用

本地创建文件夹，开始下载。

![OSSBrowser 下载](https://image.ericzzz.com/2023/09/05/aed68098-871d-47b0-a1e9-036c119c3649.png)

## Rclone

Rclone 是一个开源，多线程，命令行界面的计算机程序，可用于管理云存储。其功能包括档案同步，文件传输，加密，缓存和挂载。Rclone 支持包括 Amazon S3 和 Google 云端硬盘在内等共五十多种云存储服务。

[下载地址](https://rclone.org/downloads/)

### [创建 CloudFlare R2 Access Key](https://developers.cloudflare.com/r2/api/s3/tokens/)

- 在**帐户主页**中，选择** R2**。
- **在帐户详细信息**下，选择**管理 R2 API 令牌**。
- 选择**创建 API 令牌**。
- 选择** R2 令牌**文本以编辑您的 API 令牌名称。
- 在** Permissions **下，为您的令牌选择权限类型。我们选择最高权限`Admin Read and Write`。有关每个选项的信息，请参阅 [权限](https://developers.cloudflare.com/r2/api/s3/tokens/#permissions)。

![CloudFlare R2 权限](https://image.ericzzz.com/2023/09/05/95b24245-b86e-4b22-8385-a562a78f4c19.png)

- （可选）如果您选择**对象读写**或**对象读取**权限，则可以将令牌范围限定为一组存储桶。
- 选择**创建 API 令牌**。

### 配置 Rclone config
使用命令行，进入 Rclone 目录。
		
```shell
rclone config file # 该命令会告诉你，配置文件保存在哪里
```

创建配置文件 rclone.conf，我的文件目录是：`C:\Users\pc\AppData\Roaming\rclone\rclone.conf`，编辑文件并保存。
	
```config
[r2demo]
type = s3
provider = Cloudflare
access_key_id = YOUR_KEY
secret_access_key = YOUR_ACCESS_KEY
endpoint = https://<accountid>.r2.cloudflarestorage.com
acl = private
```

`access_key_id`：填入第一步申请的`Access Key ID`。
`secret_access_key`：填入第一步申请的`Secret Access Key`。
`endpoint`：accountid 在下图的位置。

![accountid](https://image.ericzzz.com/2023/09/05/cb37b286-19f6-4c55-9d9b-05ee47f4b70f.png)

测试，配置是否正确

```shell
rclone lsf r2demo: #查看所有 buckets
```

```shell
rclone lsf r2demo: #查看具体 bucket 
```

同步本地目录或文件到远端 bucket
			
```shell
rclone sync <LOCAL_PATH> target:bucket-name/target-path/ --progress 
```

`LOCAL_PATH`：之前 OSSBrowser 下载目录。
`target`：r2demo。
`bucket-name`：CloudFlare R2 上创建的桶，例：image。
`target-path`：可以不填。
`--progress`： 显示迁移的进度及校验的结果。

![rclone 同步](https://image.ericzzz.com/2023/09/05/8f43be60-eb14-4818-9d30-8fd7a685b59f.png)

## 替换博客中图片地址

以下操作，记得备份原文件，建议使用`Git`管理文件。

```shell
sed -i "s/miasanmia.oss-cn-beijing.aliyuncs.com\/picture/image.ericzzz.com/g" `grep -rl 'miasanmia.oss-cn-beijing.aliyuncs.com/picture' /workspaces/my-blog/content/zh-cn/posts/`
```

使用`sed`命令可以进行字符串的批量替换操作，以节省大量的时间及人力。

使用格式如下：

```shell
sed -i "s/oldstring/newstring/g" `grep oldstring -rl path`
```

`oldstring`：待替换的字符串。
`newstring`：替换之后的新字符串。
`grep`命令，按照所给的路径查找`oldstring`，`path`是查找替换文件的路径。
`-i`：直接在文件中替换，不在终端输出。
`-r`：在`path`中的目录递归查找。
`-l`：输出所有匹配到`oldstring`的文件。

**注意：如果你的文件名中带有空格，需要把文件名修改为不带空格。**
