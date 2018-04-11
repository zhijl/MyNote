# Java 实用代码

## 读取本地文件并转换为 byte 数组

``` java
import java.io.InputStream;
import java.io.FileInputStream;
import java.io.ByteArrayOutputStream;

    private byte[] InputStream2ByteArray(String filePath) throws IOException {
        InputStream in = new FileInputStream(filePath);
        byte[] data = toByteArray(in);
        in.close();
        return data;
    }

    private byte[] toByteArray(InputStream in) throws IOException {
        ByteArrayOutputStream out = new ByteArrayOutputStream();
        byte[] buffer = new byte[1024 * 4];
        int n = 0;
        while ((n = in.read(buffer)) != -1) {
            out.write(buffer, 0, n);
        }
        return out.toByteArray();
    }
```

> `toByteArray` 方法中的 `buffer` 需要修改为合适大小
