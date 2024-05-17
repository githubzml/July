### 2.5.2 FileReder 对象

FileReader 对象的常用方法

| 方法名称                   | 参数           | 描述                      |
| :------------------------- | :------------- | :------------------------ |
| readAsBinaryString(file)   | file           | 将文件读取为二进制字符串  |
| readAsText(file, encoding) | file, encoding | 将文件读取为文本格式      |
| readAsDataURL(file)        | file           | 将文件读取为 DataURL 格式 |
| abort()                    | 无             | 取消读取操作              |

File FileReader Blob ArrayBuffer 有什么关联？？？

FileReader 对象的常用事件

| 事件名称    | 描述                                   |
| :---------- | :------------------------------------- |
| onabort     | 读取操作被中断                         |
| onerror     | 读取发生错误时触发                     |
| onloadstart | 读取开始时触发                         |
| onprogress  | 正在读取时触发                         |
| onload      | 读取完成时触发                         |
| onloadend   | 读取操作完成时触发（无论成功或者失败） |

读取图片内容显示缩略图

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <input type="file" multiple />
    <img src="" alt="缩略图" />
    <script>
      const input = document.querySelector("input");
      input.onchange = function () {
        const file = this.files[0];
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = function () {
          const img = document.querySelector("img");
          img.src = this.result;
        };
      };
    </script>
  </body>
</html>
```
