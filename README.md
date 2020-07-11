### face-tracking

![https://img.shields.io/badge/license-Apache%202.0-blue.svg?longCache=true&style=flat-square](https://img.shields.io/badge/license-Apache%202.0-blue.svg?longCache=true&style=flat-square)
![https://img.shields.io/badge/vue-2.6.10-orange.svg?style=flat-square](https://img.shields.io/badge/vue-2.6.10-orange.svg?style=flat-square)
![https://tokei.rs/b1/github/chachae/face-tracking?category=lines](https://tokei.rs/b1/github/chachae/face-tracking?category=lines)

一个简单的基于 trakcing.js 实现的人脸识别案例，使用 vue-cli 完成本案例。 由于百度上的案例实在不多，能找到的案例功能实现残缺基本不符合预期，GayHub 上的 Demo 试了几个都有不能关闭摄像头的问题，且 trakcing 自带的摄像头关闭方法也并不好事，故重新写了一个。

### 功能实现
- [x] 自动人脸识别
- [x] 识别成功自动关闭摄像头
- [x] 重新识别
- [x] 人脸 Base64 编码

### 截图
<table>
<tr>
    <td align="center" style="background: #fff"><b>face-tracking</b></td>
  </tr>
  <tr>
    <td align="center" style="background: #fff"><img src="images/snipaste.jpg"/></td>
  </tr>
</table>

### 项目打包

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

### License

```reStructuredText
Copyright [2020] [chachae]

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```