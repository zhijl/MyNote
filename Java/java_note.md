## 实现图片 Base64 编码的相互转换

``` java
import java.io.InputStream;
import java.io.OutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;

class Test {

	/**
	 * @Description: 根据图片地址转换为base64编码字符串
	 * @Author: 
	 * @CreateTime: 
	 * @return
	 */
    public static String ImageBase64(String imgFile) {
	    InputStream inputStream = null;
		byte[] data = null;
		try {
			inputStream = new FileInputStream(imgFile);
			data = new byte[inputStream.available()];
			inputStream.read(data);
			inputStream.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
		// 加密
		BASE64Encoder encoder = new BASE64Encoder();
		return encoder.encode(data);
	}


	/**
	 * @Description: 将base64编码字符串转换为图片
	 * @Author: 
	 * @CreateTime: 
	 * @param imgStr base64编码字符串
	 * @param path 图片路径-具体到文件
	 * @return
	*/

	public static boolean GenerateImage(String imgStr, String path) {
		if (imgStr == null)
			return false;
		BASE64Decoder decoder = new BASE64Decoder();
		try {
			// 解密
			byte[] b = decoder.decodeBuffer(imgStr);
			// 处理数据
			for (int i = 0; i < b.length; ++i) {
				if (b[i] < 0) {
					b[i] += 256;
				}
			}
			OutputStream out = new FileOutputStream(path);
			out.write(b);
			out.flush();
			out.close();
			return true;
		} catch (Exception e) {
			return false;
		}
	}


	public static void main(String[] args) {
		String strImg = Test.ImageBase64("outdoor_002.png");
//		System.out.println(strImg);
		Test.GenerateImage(strImg, "86619-107.png");
	}
}
```

## 命令行添加依赖包运行和类打包

``` shell
# xxx.jar 为依赖包
javac -cp xxx.jar test.java
java -cp xxx.jar test.java

# 将当前目录下的所有 *.class 封装成 jar 包
javac -cf xxx.jar *.class
```

1. [Java 处理图片 base64 编码的相互转换]https://www.cnblogs.com/libra0920/p/5754356.html()