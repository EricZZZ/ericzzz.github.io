# 优化 Blog 图片访问


## 什么是 Webp？
WebP 是一种现代**图片格式**，可为网络上的图片提供出色的**无损和有损**压缩。使用 WebP，网站站长和 Web 开发者可以创建更小、更丰富的图片，从而提高网页加载速度。与 PNG 相比，WebP 无损图片的尺寸 [缩小了 26%](https://developers.google.com/speed/webp/docs/webp_lossless_alpha_study?hl=zh-cn#results)。WebP 有损图片比采用等效 [SSIM](https://en.wikipedia.org/wiki/Structural_similarity) 质量指数的同等 JPEG 图片 [小 25-34%](https://developers.google.com/speed/webp/docs/webp_study?hl=zh-cn)。

## 通过 Rclone 工具，CloudFlare R2 同步到本地
Rclone 工具相关配置，请看 [这篇文章](https://ericzzz.com/alibaba-cloud-oss-was-migrated-to-cloudflare-r2/#rclone)。

```shell
./rclone.exe sync r2demo:image E:\tools\rclone-v1.63.1-windows-amd64\down_pic\ --progress
```

通过 Rclone，把 CloudFlare R2 `image`桶中的文件全部同步到本地。

## 格式转换为 webp 格式
使用 Python，Pillow 库，把`.jpg`，`.jpeg`，`.png`格式转换成`.webp`，并删除替换之前的图片。

```python
import os
import sys
from PIL import Image

def convert_to_webp(input_file, output_file, quality=80):
    # 函数用于转换一个图片文件
    # input_file: 输入的图片文件路径
    # output_file: 输出的 webp 文件路径
    # quality: webp 文件质量，取值范围 1~100，默认值 80
    try:
        # 打开输入图片文件
        with Image.open(input_file) as im:
            # 保存为 webp 格式
            im.save(output_file, "webp", quality=quality)
            print(f"Converted: {input_file} => {output_file}")
            # 删除原来的图片文件
            os.remove(input_file)
    except Exception as e:
        print(f"Error converting file: {input_file}")
        print(str(e))

def process_folder(folder_path):
    # 函数用于处理整个文件夹
    # folder_path: 文件夹路径
    for root, dirs, files in os.walk(folder_path):
        for filename in files:
            # 检查文件后缀是否为 jpg/jpeg/png
            if any(filename.lower().endswith(ext) for ext in [".jpg", ".jpeg", ".png"]):
                input_file = os.path.join(root, filename)
                output_file = os.path.splitext(input_file)[0] + ".webp"
                # 调用转换函数
                convert_to_webp(input_file, output_file)

if __name__ == "__main__":
    # 获取用户输入
    folder_path = input("请输入文件目录：")
    # 判断用户输入是否为一个正确的文件目录
    if os.path.isdir(folder_path):
        process_folder(folder_path)
    else:
        print("请输入一个正确的文件目录")
        sys.exit(1)
    # process_folder(folder_path)
	  
```
执行完脚本，把替换好的文件目录，修改目录名，我这里改成了`down_pic_re`，一会儿需要把整个目录上传到 CloudFlare R2。

## 通过 rclone 工具，上传到 CloudFlare R2
**执行本操作之前，一定要把 CloudFlare 上需要替换的图片文件目录进行备份。**

**执行本操作之前，一定要把 CloudFlare 上需要替换的图片文件目录进行备份。**

**执行本操作之前，一定要把 CloudFlare 上需要替换的图片文件目录进行备份。**

```bash
./rclone.exe sync E:\tools\rclone-v1.63.1-windows-amd64\down_pic_re\ r2demo:image --progress
```

![替换后的图片同步到 R2](https://image.ericzzz.com/2024/01/31/763e5b0b-c689-4938-9e69-5eecb8456cc9.webp)

## 修改 Blog 中图片的后缀

批量操作，把所有文章中的出现的`.jpg)`，`.jpeg)`，`.png)`字符串，替换成`.webp)`

```shell
sed -i -E 's/(\.jpg\)|\.jpeg\)|\.png\))/.webp\)/g' `grep -rlE '\.jpg)|\.jpeg)|\.png)' /workspaces/my-blog/content/zh-cn/posts/`
```

`sed` 命令参数说明
- `-i`：参数用于直接修改文件。
- `-E`：参数表示使用扩展正则表达式。
- `s/(\.jpg\)|\.jpeg\)|\.png\))/.webp\)/g`：是替换的模式，它会将所有匹配的 `.jpg`，`.jpeg`，和 `.png` 替换为 `.webp`。
  
`grep`命令参数说明
- `-r`：递归地在指定目录及其子目录中搜索文件。
- `-l`：直接在文件中替换，不在终端输出。
- `-E`：启用扩展的正则表达式语法。

## 完成

对比优化之后桶的大小，高下立判，访问 Blog 文章的速度是不是快了一丢丢😎。

- 优化前
![优化前](https://image.ericzzz.com/2024/01/31/73edb196-18b7-428a-ba9d-9acb91b7cb98.webp)

- 优化后
![优化后](https://image.ericzzz.com/2024/01/31/99b318ef-bdb9-42ff-a82b-91e993be78a2.webp)

## 参考资料
[Python 把图片转换为 webp](https://blog.xpdbk.com/post/python-img-to-webp/)

[webp](https://developers.google.com/speed/webp?hl=zh-cn)
