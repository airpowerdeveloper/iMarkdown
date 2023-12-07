**项目地址**

https://github.com/CorentinTh/it-tools

**安装准备**

全局安装pnpm

```
npm install pnpm -g
```

'vue-tsc' 不是内部或外部命令，也不是可运行的程序或批处理文件。

```
npm install -g @vue/cli
npm install -g typescript
npm install -g vue-tsc
npm install -g tslint
```

'NODE_OPTIONS' 不是内部或外部命令，也不是可运行的程序或批处理文件。

```
npm install -g win-node-env
```

'vite' 不是内部或外部命令，也不是可运行的程序或批处理文件。

```
npm install -g vite
```

**安装部署**

可以通过Docker镜像运行，镜像源有两个：

从Docker Hub拉取镜像
docker run -d –name it-tools –restart unless-stopped -p 8080:80 corentinth/it-tools:latest

从Github packages
docker run -d –name it-tools –restart unless-stopped -p 8080:80 ghcr.io/corentinth/it-tools:latest

当然，大家也可以拉下来，自己运行`npm run build`,将`dist`中的文件上传到服务器或者OSS中



项目目录执行

```
npm install

npm run build
```



**主要功能**

**Crypto工具类**

生成token，哈希文本，生成UUID，文本加解密等10种功能

**Converter工具类**

Yaml 转化器，Json转化器，Base64字符串编码等15种功能

**Web工具类**

JSON对比，URL编码解码等15种功能

**Images and videos工具类**

SVG 占位符生成器，普通二维码生成器，WiFi二维码生成器等4种功能

**Development工具类**

SQL美化与格式化，JSON美化格式化，Docker run转Docker compose等10种不同功能

**Network工具类，Math工具类，Measurement工具类，文本工具类，数据工具类等**