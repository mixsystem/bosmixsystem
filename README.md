![cover](https://github.com/mixsystem/bosmixsystem/blob/master/images/cover.jpg)
# MixSys：An Anonymous Agency Transfer System

*摘要：Mixsys以信息加密和混淆传输技术为基础，将透明的区块链代币传输变得难以追踪，以解决目前区块链所面临的隐私暴露问题。*

### 背景

随着区块链技术的商业发展，很多分布式应用对公链提出了更高的性能要求。以EOS为代表的公链系统在解决TPS问题的同时，隐私保护却被削弱了，公链面临的矛盾是：公链的高性能是以降低“去中心化”为代价来实现的，越是高性能的公链，越是偏向中心化。例如，目前BTC的全节点达到了2万个左右，ETH的节点在2000左右，EOS的节点在21个。这显示出，TPS越高的公链，中心化程度越高。 

|  Blockchain  | Nodes  | 中心化程度 |
| --- | --- | --- |
| BTC  | 30000+ | 极低  |
| ETH  | 2000+ | 低     |
| EOS  | 21 | 高   |

**中心化和高性能的关系如下图所示**：

![center](https://github.com/mixsystem/bosmixsystem/blob/master/images/center.png)

公链偏向中心化，随之而来的问题就是，资产的安全和个人隐私受到威胁。MixSys采用信息加密和混淆传输技术，致力于解决公链传输中的隐私问题。

### 基于EOS的MixSys

2017年，在Daniel Larimer的主导下，Blockone设计并开发了EOS区块链，自2018年6月上线以来，EOS已迅速在主流应用和商业用途中传播开来，成为首个高性能的公有区块链。

EOS提供的转账交易，可以永远保存在全网上。然而，一个重要的问题是，这种交易方式，仅仅只通过账户名这种假名交易提供了低层次的隐私保护，对隐私要求更高的交易行为，EOS默认的传输方式（Transfer）难以提供更多的保护。

MixSys是首个以保护隐私为要旨的传输混淆系统。基于EOS的智能合约及对称加密技术，Mixsys在EOS的传输基础上进行了一系列的改进，由此诞生出一个去中心化的和具备良好匿名性的混淆传输系统，它能够完成交易的同时，使得发送方和接收方不再具有明显的关联性。我们相信，提高交易的隐私程度，这对于解决EOS过于透明和中心化问题来说，是至关重要的。MixSys致力于通过混淆EOS的传输记录，保护操作者的隐私。

### 匿名机制

对于交易隐私的保护，通常采取的是特征泛化和特征隐匿，在EOS上，由于客户发送代币到合约是一个被动的过程，合约无法再次操作客户账号，所以，在EOS上无法支持从发送方来实现的特征泛化，继而，MixSys采用的是特征隐匿的方式保护交易隐私。

**特征隐匿的混淆机制**

由于交易发送方无法被隐匿，所以MixSys对交易接受方的信息做混淆和隐匿，基于有限域中多项式的解，来实现订单切割，实现顺序混淆和时域混淆的隐私安全机制。从整体上增强传输系统的隐私性。

MixSys使用5.0EOS，50EOS和200EOS为传输分割单位。在每轮混币过程中，所有用户应该以相同面值的形式输入和输出资金。所有交易会分解成分散的、独立的、前后没有关联的定额交易，同时MixSys混淆系统在队列和时间上进行随机混淆。

由于采用了小型定额交易，对于大资金来说，MixSys的混淆机制需要多轮混币，MixSys以被动的模式运行，每轮混淆过程可视为增强用户资金匿名性的独立事件。在初期，每轮混淆只限制5个参与者，因此追踪者有五分之一的机会追踪交易，为了提高匿名的质量，混淆系统并不会固定哪个订单进入混淆池，这可以使追踪者低于20%的几率追踪到交易的真实信息。

你将会看到，MixSys传输一次交易，可能是一笔等待了10分钟的混淆订单，也可能是前一秒的订单。发送时间的随机化再次使得混淆度剧烈增加，导致无法追踪实际交易。

假定给出的数字是P，K:上标是第n次混淆队列，下标是混淆组别,将P分解为多项式如下：
$$
P>200EOS:
$$

$$
P=K_2^1*a+K_1^2*b+K_1^3*c+...+K_1^n*n
$$

a/b/c/d...取值范围0-99,当取值为0，表示不参与这次混淆，实现了一笔转账交易在混淆队列内部不是均匀分布，在时间上不可预测，极大的增强了混淆度。

- 混淆前： 
```mermaid
graph TB
subgraph EOS提供的透明传输交易
1--明文传输-->A
2--明文传输-->B
3--明文传输-->C
4--明文传输-->D
5--明文传输-->F
end
```

- 混淆后： 
```mermaid
graph TB
subgraph MixSys混淆转账交易
1--转账-->主合约
2--转账-->主合约
3--转账-->主合约
4--转账-->主合约
5--转账-->主合约
end
subgraph 
主合约--转账轮次K1-->E
主合约--转账轮次K3-->B
主合约--转账轮次K1-->D
主合约--转账轮次K2-->C
主合约--转账轮次K2-->A
end
```
由MixSys主合约批量发送转账队列K1..K2..K3...Kn，转账的发起地址和目标地址已被混淆，时间对应已被混淆，同时达成了目标混淆和时间混淆。

### 混淆度(blend)
 **混淆度定义**：混淆系统输出交易后，任一目标地址和真实目标地址的重合率，重合率以100%为模的补数为混淆度，取值范围0%-100%。

**影响混淆度的几个因素**
1.  随着MixSys混淆系统的运行，越来越多的交易被加入队列混淆，MixSys运行时间和混淆度成正相关。
2.  算法的均匀度与客户的随机性，保持了不被统计学规律攻击。
3.  订单的拆分机制迭代，适用于EOS的传输场景，增强混淆度。

### AES+RSA混合算法

通过Web+Scatter前端，用户提交转账请求，当目标账户以明文形式写入MEMO，则此交易信息在EOS链上公开化，此时MixSys引入RSA加密将目标账户MEMO加密上链。

- 预设RSA初始密钥，长度32位，每隔24小时使用MD5迭代密钥。
- 将交易目标帐号使用RSA加密写入MEMO。

> ```
> <script type="text/javascript">
>         function crypto() {
>             var passwd = $('#passwd').val();
>             var rsa = new RSAKey();
> 　　　　　　　//openssl十六进制数据
>             var modulus = "DB1EA572B55F5D9C8ADF092F5DCC3559CFEA8CE8BB54E3A71DA9B1AFBD7D17CF80ADB224FE4EA5379BC782F41C137748D8F1B5A36AD62A127EF5E87EFB25C209A66BCEE9925CE09631BF2271E81123E93438646625080FF04F4F2CF532B077E3E390486DF40E7586F0AE522C873F33170222F46BDB6084F55DE6B7031E55DBE7";
>               //openssl e值(0x10001)
> 　　　　　　　var exponent = "10001";
>             rsa.setPublic(modulus, exponent);
>             var res = rsa.encrypt(passwd);
>             $('#passwd').val(res);
>         }
>     </script>
> 	</head>
> <body>
> <form action="./sent.php" method="post" >
>     <label for="memo">目标账户：</label><input type="text" name="memo" id="memo">
>     <input type="submit" value="发送" onclick="crypto();">
> </form>
> </body>
> </html>
> ```
- 后端使用PHP解密

> ```
> //私钥
> $private_key = "-----BEGIN RSA PRIVATE KEY-----
> MIICXAIBAAKBgQDICrXl+ELhqFE0xCGA3/xdN1VxwUc5kxAKb5f3F28HY40zdXbnuwpdQNKnHwTNvHZupBRHlYpqX+1Plti81sRadtYKNwVoLRlwwP3mzIyRRvO0hf3cVXn33r4A5y1YUh+txvfgikbmmnsjP4pP3CIZC0TeDMt1+h17cV6s6KDdOQIDAQABAoGAdu2/NQkDIkvCc7+DN65yyCyklOvW34jqSXoM++8nqFnsUcKtPZn60HRAiPEtFu/L7X2JPQHP9+wuF1pUYvjAZQb5E9n8G12nxduBS5Ux085INOWP69pXnsljhyipmpRgPf7ZsgGlftVXlg+HrUfzxj26NqEKwn61mi+CjMSMt/ECQQDpg868PEiJED/gpUMTT7gRgDAdmPJ0+ALn82zsfA84zDA8Po9EREfXRM2Mar/fuvXL/vc0FBRYGaA/VAinKJ6tAkEA203JDovTnfG87zNl9z0/z3kUxCMRy+7M28+7btF1eaDzxJWVFWtXcv4EjHjDN6ClhT6ATuamw0sNlta9n4cGPQJASBltXItUbhV5U5hTNIQ5thWZRZoyylDFPO7ZI89ON+A7yG4ceByI/ft9Z6ySH7cvK1RG6WqFuYn4A3Wj0lDlZQJBAIjrPq0TRbLl25ToXTAIkI9jREDUuGa/mC1gu+ALbDHc+HTzAxpVY0rLJ2Hgf9l6Nf1DsW4ReWqSsFBNkCRKc+0CQClxcPv2wwSGMFhLTqpKeySPIeVy4NrTHv8zEYvm7MuUGvlsj6sqvcuUQbVHoLRpqQiDi8TPaEMHzS9fhDjVUUc=
> -----END RSA PRIVATE KEY-----";
> 
> $hex_encrypt_data = trim($_POST['password']);  //十六进制数据
> $encrypt_data = pack("H*", $hex_encrypt_data); //对十六进制数据进行转换
> openssl_private_decrypt($encrypt_data, $decrypt_data, $private_key); //解密数据
> echo '解密后的数据：' . $decrypt_data;
> ```

### 合约架构

- **收银员账户**，这是一个权限被主合约控制的EOS账号，它本身没有合约部署，不会被攻击。仅仅负责收款。前端Web+Scatter提交转账请求后，EOS币被转入收银员账号，转账目标账号被加密写入转账MEMO。
- **解密与发送交易主控合约**部署在EOS主网上，静默工作模式，他不接受除“传达员账号”外的其他任何账号发送的信息。没有对外接口提供，不会被调用和激活。他仅仅解密传达员账号发送过来的目标密文信息。
- **传达员账户**，这个账户运行在php后端，没有合约部署，也没有资金，仅仅用来发送密文给主合约。传达员账户将会24小时为周期更换公钥，防止被破解。
- **混淆员账户**，由主合约控制的混淆账户，由于数量众多，所以它们当中的每个账户仅仅偶尔随机参与混淆。  

```mermaid
graph TB
    subgraph 加密
    目标账户明文--RSA加密-->MEMO
    MEMO--EOS transfer-->收银员账户
    end
    subgraph 传送
   传达员账户--MEMO已混淆--> 主传送合约
    end
    subgraph 
    收银员账户--权限控制-->主传送合约
    end
    subgraph 接收
    主传送合约--> 混淆池账户A
    end
    subgraph 接收
    主传送合约--> 混淆池账户B
    end
      subgraph 接收
    主传送合约--> 混淆池账户C
    end
    subgraph 
    混淆池账户A--> 目标账户
    end
     subgraph 
    混淆池账户B--> 目标账户
    end
     subgraph 
    混淆池账户C--> 目标账户
    end
    subgraph 解密
    后端PHP--RSA解密并混淆MEMO--> 传达员账户
    end
    subgraph 
    收银员账户--RSA解密MEMO--> 后端PHP
    end
```
### 在EOS上使用MixSys

使用Chrome打开 https://eos.mixsystem.network/ 登录scatter，选择目标账户，输入数量，写入备注，点击转账发送交易。

请注意：
- 不支持通过Mixsys玩游戏。
- 因没有Token回传机制，所以不支持参与其他项目的ICO。
- 支持转账给交易所，但不支持转账给大部分的智能合约。


### 转账耗时与费用
|
###### 表2：转账周期是Action最长时间，通常会小于这个数值。

### 结论
本白皮书介绍了MixSys混淆系统的设计模型，这对于EOS用户来说意味着，传输有了更好的隐私性。MixSys去中心化混淆技术，极大的增强了EOS区块链的隐私保护。

### 参考
1. [A Peer-to-Peer Electronic Cash System（2008）](https://bitcoin.org/bitcoin.pdf)
2. [Dash: Una Divisa Cifrada Centrada en la Privacidad](https://github.com/dashpay/dash/wiki/Whitepaper_es)
3. [EOSIO - The Most Powerful Infrastructure for Decentralized Applications](https://github.com/EOSIO/Documentation/blob/master/TechnicalWhitePaper.md)
4. [EOSIO developers C++ API reference document](https://developers.eos.io/eosio-cpp/v1.3.1/docs/c-cpp-api)
5. [Bancor Protocol Contracts](https://github.com/bancorprotocol/contracts)

---

###### 未经允许，禁止任何人复制或发布本白皮书内的任何内容。
Copyright © 2019 Mixsys.network
