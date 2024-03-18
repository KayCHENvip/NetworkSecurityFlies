**VS2019编译Masscan**

去github克隆最新源码: [https://github.com/robertdavidgraham/masscan](https://github.com/robertdavidgraham/masscan)

进入 VS2019，打开 `.sln`

升级到 VS2019

进入 `Source Files`，找到 `misc` 文件夹，找到 `string_s.h`，找到第58行

将 `== 1900` 改成 `>=1900` 就可以了

![Image 1](https://img2020.cnblogs.com/blog/1241541/202111/1241541-20211105155804236-410749391.jpg)

![Image 2](https://img2020.cnblogs.com/blog/1241541/202111/1241541-20211105160113854-1306537484.jpg)

![Image 3](https://img2020.cnblogs.com/blog/1241541/202111/1241541-20211105160122713-303671754.jpg)

