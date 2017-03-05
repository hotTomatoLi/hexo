---
title: spring-boot-vue-webpack开发项目搭建
tags: [springboot-vue-webpack]
toc: true
---

# 采用技术
- springboot
- vuejs
- gradle
- webpack
- idea + sublime
最终得到springboot作为restful的服务端，vuejs作为展示端的项目，并以此为基础，进行项目开发。开发工具采用gradle作为
构建工具，idea作为java端开发工具，sublime作为展示端开发工具。

# 搭建过程
## springboot
### 新建
idea下新建springboot项目。

![springboot项目](http://upload-images.jianshu.io/upload_images/1213316-448cdf0c600a47e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 增加子项目

![项目结构](http://upload-images.jianshu.io/upload_images/1213316-182f467e5dd1fdb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
根目录下的setting.gradle中
```
rootProject.name = 'spring-boot-webpack-vue'
include 'server'
include 'app'
```
根目录下的build.gradle
```
buildscript{
    repositories {
        mavenCentral()
        maven {
            url 'https://plugins.gradle.org/m2/'
        }
    }

    dependencies {
        classpath 'org.springframework.boot:spring-boot-gradle-plugin:1.4.0.RELEASE'
        classpath 'com.moowork.gradle:gradle-node-plugin:0.13'
    }
}

allprojects {

    group 'com'
    version '1.0-SNAPSHOT'

    apply plugin: 'java'
    apply plugin: 'idea'

    sourceCompatibility = 1.8

    repositories {
        mavenCentral()
    }
}
```

### spring boot项目
结构

![spring boot项目](http://upload-images.jianshu.io/upload_images/1213316-abb846320b1830b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

build.gradle
```
project(':server') {

    apply plugin: 'spring-boot'

    dependencies {
        compile project(':app')
        compile 'org.springframework.boot:spring-boot-starter-web:1.4.0.RELEASE'
    }
}
```

WebConfiguartion.java
```
@Configuration
@RestController
public class WebConfiguration extends WebMvcConfigurerAdapter {

    @Value("classpath:/app/index.html")
    private Resource indexHtml;

    @Bean
    @Order(Ordered.HIGHEST_PRECEDENCE)
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter("UTF-8", true);
        return filter;
    }

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/**").addResourceLocations("classpath:/app/");
    }

    @Bean
    public ServletRegistrationBean apiV1ServletBean(WebApplicationContext wac) {
        DispatcherServlet servlet = new DispatcherServlet(wac);
        ServletRegistrationBean bean = new ServletRegistrationBean(servlet, "/api/v1/*");
        bean.setName("api-v1");
        return bean;
    }

    @RequestMapping("/")
    public Object index() {
        return ResponseEntity.ok().body(indexHtml);
    }
```
SampleController.java
```
@SpringBootApplication
@EnableAutoConfiguration
public class SampleController {


    public static void main(String[] args) throws Exception {
//        SpringApplication.run(SampleController.class, args);
        new SpringApplication(SampleController.class).run(args);
    }

}
```
运行，即可启动。

### 展示端项目
展示端项目需要安装nodejs，以及webpack。
执行
```
spring-boot-webpack-vue>vue init webpack app
```
然后执行
```
spring-boot-webpack-vue\app>cnpm install
```
安装完后，需要进行以下修改，主要目的是可以利用gradle命令将展示端脚步发布到spring项目目录下。
#### 修改build目录
将build目录重命名为build-scripts，同时修改app目录下package.json
```
  "scripts": {
    "dev": "node build/dev-server.js",
    "build": "node build/build.js",
    "unit": "cross-env BABEL_ENV=test karma start test/unit/karma.conf.js --single-run",
    "e2e": "node test/e2e/runner.js",
    "test": "npm run unit && npm run e2e",
    "lint": "eslint --ext .js,.vue src test/unit/specs test/e2e/specs"
  },
```
修改后
```
  "scripts": {
    "dev": "node build-scripts/dev-server.js",
    "build": "node build-scripts/build.js",
    "unit": "cross-env BABEL_ENV=test karma start test/unit/karma.conf.js --single-run",
    "e2e": "node test/e2e/runner.js",
    "test": "npm run unit && npm run e2e",
    "lint": "eslint --ext .js,.vue src test/unit/specs test/e2e/specs"
  },
```

修改app\build-scripts目录下webpack.dev.conf.js文件
```
Object.keys(baseWebpackConfig.entry).forEach(function (name) {
  baseWebpackConfig.entry[name] = ['./build/dev-client'].concat(baseWebpackConfig.entry[name])
})
```
修改后
```
Object.keys(baseWebpackConfig.entry).forEach(function (name) {
  baseWebpackConfig.entry[name] = ['./build-scripts/dev-client'].concat(baseWebpackConfig.entry[name])
})
```

#### build.gradle
```
project(':app') {

  apply plugin: 'com.moowork.node'

  task cnpmInstall(type: NpmTask) {
    group = 'node'
    args = ['install', '--registry=http://registry.cnpmjs.org']
  }

  task buildUI(type: NpmTask, dependsOn: cnpmInstall) {
    group = 'node'
    args = ['run', 'build']
  }
  jar.dependsOn buildUI

  task runDev(type: NpmTask, dependsOn: cnpmInstall) {
    group = 'node'
    args = ['run', 'dev']
  }
}
```

#### index.js
修改app\config目录下的index.js
```
// see http://vuejs-templates.github.io/webpack for documentation.
var path = require('path')

var assetsRoot = path.resolve(__dirname, '../build/resources/main/app')

module.exports = {
  build: {
    env: require('./prod.env'),
    index: path.resolve(assetsRoot, 'index.html'),
    assetsRoot: assetsRoot,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    productionSourceMap: true,
    // Gzip off by default as many popular static hosts such as
    // Surge or Netlify already gzip all static assets for you.
    // Before setting to `true`, make sure to:
    // npm install --save-dev compression-webpack-plugin
    productionGzip: false,
    productionGzipExtensions: ['js', 'css'],
    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  },
  dev: {
    env: require('./dev.env'),
    port: 3000,
    autoOpenBrowser: true,
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/api/**': 'http://localhost:8080'
    },
    // CSS Sourcemaps off by default because relative paths are "buggy"
    // with this option, according to the CSS-Loader README
    // (https://github.com/webpack/css-loader#sourcemaps)
    // In our experience, they generally work as expected,
    // just be aware of this issue when enabling this option.
    cssSourceMap: false
  }
}

```

#### 运行
在app目录下执行
```
npm run dev
```
