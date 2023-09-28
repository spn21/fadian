新协议：

允许客户在不泄露信息的情况下了解其用户名和密码是否出现泄露。

主要优势：

1.考虑敌对客户端/服务器的威胁，使用计算高成本散列，匿名性和私有集交集的组合解决；

2.上述隐私要求能够根据被破坏的凭据数据库检查客户的确切用户名和密码，减少误报。

2.1 abstract protocol

客户端$（u，p）$通过$CreateRequest（u，p）$进行计算生成$Local State$和$Request$；

服务器收到$request$，访问凭证存储$Store$，通过$CreateResponse（S，Req）$，将生成的Response发送到客户端；

客户端计算$Verdict（Resp，LS）$判断所查询凭证是否因违规而暴露。

2.2设计原则

Democratized access:不能依赖经过身份验证的账户作为速率限制的一种形式

Actionable, not informationa

Breached, not weak

Near real-time:查询凭证和了解违规状态间隔应接近实时

2.3 Threat model

考虑对抗性客户端与对抗性服务器

if：恶意客户端访问自己违规数据集$D= {（u_1，p_1），...，（u_n，p_n）}$；

攻击者试图学习$u∈S-D$；$p∈S-D(eg.添加到破解字典中的新密码)$；新的密码凭证$（u,p)∈S-D$；

恶意服务器可能获取客户端$u ,p$

解决：

匿名集K

Appendix A 说明

Requester credential anonymity:如果对于每个凭证 $(u1, p1)$，存在一个足够大的包含$ (u1, p1)$ 的匿名集 $K$，使得 $∀(u2, p2) ∈ K$，则协议提供请求者凭证匿名性： $CreateRequest(u1, p1) ) ≈ CreateRequest(u2, p2)$。 

Response with bounded leakage:改写安全概念：对于任意$（u_1，p_1），（u_2，p_2）∈k， 使[(u_1, p_1) ε S] = [(u_2, p_2) ε S]: CreateResponse(S, CreateRequest(u_1, p_1)) ≈ CreateResponse(S, CreateRequest(u_2, p_2))。$

Inefficient oracle:存在时间段T，使

$t(CreateRequest(u_i,p_i))>T$（$t(f)$表示函数f运行时间）

检查凭证的成员资格：$t([(u,p)∈S]) > T'$（理想情况下T=T'）

####

2.4Trandeoffs of existing schemes

H(u)：query by username

H(p)：query by password

PasswordPing 使用SHA1,SHA256,MD5 hash的10-hex字符查询前缀

HaveBeenPwned使用SHA1 hash的5-hex字符前缀

1Password依赖HaveBeenPwned和密码前缀方法进行违规警报

query by domain				

query by username,then password

PasswordPing 提供了一种协议，客户端首先使用 SHA-256 查询 $u$或 $H(u)$，然后接收与该帐户关联的 salt 。客户端使用它通过 Argon2 计算 $H(u, p, s)$，仅发送 N 位前缀 $H(u, p, s)_{[0:N]}$

2.5 Alternative cryptographic protocols

Pricvate Information Retrieval

要求用户能够从服务器查询项目，而无需透露查询的哪个项目

Oblivious Transfer

1-out-of-N OT protocols 拓展PIR威胁模型，要求客户端在查询期间不了解有关服务器数据库中为访问元素的信息。但是开销与N（数据库条目数量）成正比

Private Set Intersection

协议分别拥有两个互不了解的S1和S2集合的两方计算$S1∩S2$（客户端有一个单例集）服务器不学习



3.Breach alerting protocol

设计依赖于k-匿名，私有集交集和散列组合

3.1Resource-constrained attacker variant

CreateDatabase

服务器规范化凭证关联用户名（算法1）

计算规范用户名和凭证密码的计算成本较高hash值

服务器通过hash映射到椭圆曲线，使用224为密钥b（用于盲化）对16节hash输出进行盲化。 NID_secp224r1并求出结果点b，服务器仅保存一个2字节非盲hash前缀，用于对数据库进行分区S'。

CreateRequest

客户重复与服务器相同的hash和盲化策略（算法2）

客户端采用自己的密钥 a，并根据请求初始化该密钥。生成的请求包括 2 字节哈希前缀和盲化完整哈希。

CreateResponse

服务器给定hash前缀（算法3）

返回与该前缀相关的所有已知不安全凭证S'

依赖DH是私有集交集，对非移动设备上的网络设置相对有效。服务器返回所有已知的被破坏的b盲凭证，同时向客户段提供双盲列表Hab的索引（ECDH的交换性质，以便客户端可以在验证过程中解密以恢复Hb，同时S’其余内容保持隐藏）

hash盲方案在DH假设下针对诚实好奇对手实现oblivious伪随机函数（OPRF）key b 保密时，$(u_i,P_i)$的散列盲方案的输出不会透露任何其他输入$（u',p')$的散列盲方案信息

Verdict：客户端通过算法4似有集交集协议来确定其凭证是否在泄露中暴露（本地实现）

3.2 Zero-password leakage variant

算法2中客户端仅计算用户名$H(u')_{[0:n]}$的hash前缀以及整个凭证的盲hash

修改算法1创建$H(u'_i)_{[0:n]}$到$H(u'_i,p'_i)$之间的映射，并使其按$H(u'_i)_{[0:n]}$划分数据库

3.3 Expansion to metadata

协议不包括有关公开凭证来源的消息作为元数据（如哪个服务收到损害）

3.4 Limitation

要求客户端能使用256MB内存计算hash值

4.Implementation

4.1 client

依靠libsodium中Argon2的js实现hash处理，依靠Openssl的Web程序集编译进行私有集交集所需的椭圆曲线计算

detecting login

依靠启发式方法检测表单是否包含用户名或密码字段，无法检测则不会向API发送内容

warning design

通过拓展弹出窗口显示状态警告

identifying user actions:用户使用泄露凭据进行身份验证时发出警告，缓存Argon2凭证hash的12字节前缀（8MB内存，仅在本地使用），以避免为同一凭证生成新的API查询，同时减少输入延迟

telemetry:使用zxcvbn进行密码强度评估

4.2 storage

将计算，盲化和散列的凭证语料库划分为216个切边，作为静态文件存储再google cloud storge中

4.3 Server

 google cloud functions

通过阻止不符合我们期望客户端提供的固定长度盲化hash的格式错误的请求来避免应用程序层拒绝服务攻击

5 Deployment

user demographics

scaling to requests

client overhead

cost modeling

6 Analysis

6.1 credential stuffing risk and risk remediation

frequency of breached credential reuse 

ignoring breached credentials

remediation of breached password

6.2 influence of domains on account security



7 Related work

account hijacking threats

password reuse behaviors

improving breach alerting protocols



8 conclusion

