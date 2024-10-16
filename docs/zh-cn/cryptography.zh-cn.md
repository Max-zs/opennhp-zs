---
layout: page
title: 加密算法
parent: 中文版
nav_order: 3
permalink: /zh-cn/cryptography/
---

# 加密算法
{: .fs-9 }

加密是 OpenNHP 的核心，通过利用尖端的加密算法提供强大的安全性、卓越的性能和可扩展性。
{: .fs-6 .fw-300 }

[English](/cryptography/){: .label .fs-4 }

---

本文解释了 OpenNHP 如何在多个关键领域中利用现代加密算法的优势：

1. [公私钥加密算法](#1-public-key-cryptography)
2. [密钥交换、数据加密和身份认证](#2-key-exchange-data-encryption-and-identity-verification)
3. [密钥分发和管理](#3-key-distribution-and-management)


## 1) 公私钥加密算法
### 1.1 简介
在不断变化的网络安全环境中，保护通信和网络资源至关重要，特别是在网络威胁日益复杂的情况下。网络基础设施隐藏协议（NHP），作为一种零信任安全机制，致力于通过隐藏网络基础设施细节来防止攻击者入侵，并确保只有受信任的实体能够与网络资源交互。NHP 安全模型的关键组件是椭圆曲线加密（ECC）用于公钥加密。在本文中，我们将探讨 ECC 如何集成到 NHP 零信任协议中，以提供强大且高效的安全性。

椭圆曲线加密（ECC）是一种现代公钥加密方法，相较于传统方法如 RSA，它能在显著更小的密钥尺寸下提供相同级别的安全性。ECC 依赖于有限域上椭圆曲线的数学特性，提供了安全性与性能之间的良好平衡。由于其降低了计算开销，ECC 特别适用于资源受限的环境，如嵌入式系统或移动设备。公钥加密是基于非对称密钥对的加密方法，通常包括一个公开的公钥和一个私密的私钥。通过公钥加密，可以实现安全的数据传输、身份验证和数字签名等功能，从而确保通信双方的数据安全性和身份真实性。

### 1.2 什么是椭圆曲线加密？

椭圆曲线加密（ECC）是一种基于椭圆曲线数学理论的公钥加密技术。它通过椭圆曲线方程上的点运算，提供与传统方法（如 RSA）相同的安全性，但密钥尺寸更小，计算效率更高。ECC 的安全性源于椭圆曲线离散对数问题，其计算复杂度使得破解难度极大。

在 ECC 中，参与方通过生成公私钥对，利用椭圆曲线 Diffie-Hellman（ECDH）协议来交换密钥。ECC 的数学基础是使用有限域上的椭圆曲线方程：

y^2 = x^3 + ax + b

其中 a 和 b 是定义曲线特性的常数，曲线上的点集具有特定的加法运算规则。

ECC 的主要优势包括：
- **更小的密钥尺寸**：ECC 能在更小的密钥长度下实现相同的安全性，从而降低了存储和计算的负担。
- **增强的安全性**：ECC 基于椭圆曲线离散对数问题，其数学复杂性使其非常难以破解。
- **高效性**：相比传统加密方法如 RSA，ECC 的加密和解密操作速度更快，适用于资源受限的设备（如嵌入式系统和移动设备）。

NHP 使用 ECC 进行密钥交换、数据加密和身份验证，以及通过无证书公钥加密（CL-PKC）进行密钥分发和管理。

### 1.3 安全通信中的 ECC

#### 1.3.1 密钥交换机制

加密密钥的安全交换是任何安全通信协议的核心。NHP 使用椭圆曲线 Diffie-Hellman（ECDH）作为其密钥交换机制。在 ECDH 密钥交换中，通信双方使用椭圆曲线生成公私密钥对，然后交换公钥，以便双方计算出共享密钥，而无需直接在网络上传递。

使用 ECDH 的好处有两个方面：首先，它提供了前向安全性，即使在未来某一方的私钥被泄露，先前建立的会话密钥仍然是安全的。其次，由于 ECC 的高效性，密钥交换过程计算负担较轻，确保密钥建立过程快速完成且计算成本低。

#### 1.3.2 数字签名身份验证

在零信任环境中，身份验证至关重要。NHP 使用椭圆曲线数字签名算法（ECDSA）来验证试图访问网络资源的实体的身份。ECDSA 是一种基于 ECC 的数字签名方案，它允许设备在不暴露敏感私钥的情况下证明其身份。

在 NHP 协议中，当某个实体希望与网络通信时，它必须使用其私钥生成数字签名，接收方可以使用相应的公钥来验证签名的有效性。这确保了只有合法的实体可以参与网络，从而有效地实施零信任模型的 "永不信任，始终验证" 原则。

#### 1.3.3 数据保密加密

NHP 在通信过程中使用对称加密来确保数据的保密性，但对称密钥必须在实体之间安全地分发和共享。ECC 通过 ECDH 提供安全的对称密钥分发渠道，确保对称密钥可以安全地交换。

一旦这些密钥交换完成，NHP 就会切换到对称加密进行数据传输，从中受益于对称加密算法的速度和效率。ECC 确保对称密钥交换既安全又高效。

#### 1.3.4 无证书公钥加密中的密钥管理与分发

NHP 还通过无证书公钥加密（CL-PKC）使用 ECC 进行密钥管理和分发。在传统的公钥基础设施中，证书被用来验证公钥，这在证书管理方面引入了复杂性。CL-PKC 通过允许实体与受信任的中心合作生成部分私钥，同时独立生成自己的密钥对，从而消除了证书的需要。

这种方法简化了密钥管理，确保公钥可以在没有证书颁发和验证的情况下安全使用。通过在 CL-PKC 中使用 ECC，NHP 提供了一种轻量级且安全的密钥分发方式，通过消除对集中式证书机构的依赖，进一步增强了零信任模型。

### 1.4 使用 ECC 的优势

NHP 零信任协议中使用 ECC 提供了众多符合其安全目标的优势：

- **可扩展安全性**：ECC 较小的密钥尺寸提供了强大的安全性，能很好地适应对抗对手计算能力不断增强的情况。随着 NHP 致力于为多样化的网络部署提供零信任环境，ECC 的可扩展性是一项关键资产。
- **资源效率**：相比传统公钥加密，ECC 减少了网络设备的计算负担。在网络资源可能受限的环境中——如边缘设备或物联网组件——这种高效性对于在不牺牲安全性的情况下保持高性能至关重要。
- **性能提升**：结合 ECDH 的密钥交换、ECDSA 的身份验证以及高效的对称加密提供了一个平衡的安全通信解决方案。这种平衡的方法使得 NHP 能够实现零信任的目标，同时保持较低的延迟，这对时间敏感的网络应用尤为关键。

### 1.5 结论

将椭圆曲线加密集成到 NHP 零信任协议中，提供了一种在最小性能影响下保护网络通信的强大手段。通过利用 ECDH 进行安全的密钥交换、ECDSA 进行可靠的身份验证，以及高效的对称加密进行数据传输，ECC 支持零信任模型的目标，即隐藏网络基础设施，确保只有受信任的实体可以访问资源，并以低开销保持安全性。

随着网络威胁变得越来越复杂，像 ECC 这样的先进加密技术在 NHP 协议中的应用对于保持对攻击者的优势至关重要。ECC 和 NHP 之间的协同作用不仅有助于保护关键的网络基础设施，还确保安全措施既强大又高效——这是任何现代网络安全项目成功的关键组合。

网络基础设施隐藏协议（NHP）基于零信任安全模型构建，确保即使在潜在攻击者的存在下也能实现安全通信。为此，NHP 集成了 Noise 协议框架，这是一个用于安全且灵活的密钥交换、数据加密和身份验证的加密框架。该组合以最小的计算开销提供了强大的安全性。

## 2）密钥交换、数据加密和身份认证

### 2.1 简介

#### 2.1.1. 密钥交换机制

NHP 利用 Noise 协议的密钥交换机制来确保通信双方之间的安全认证通道。密钥交换从握手阶段开始，双方交换 Diffie-Hellman (DH) 公钥。在 Noise 中，每一方生成一个临时密钥对，并使用交换的公钥派生出共享密钥，该共享密钥随后用于加密后续通信。

Noise 允许 NHP 支持长期静态密钥和临时密钥以增强安全性。Noise 框架的握手模式的灵活性使得 NHP 能够根据特定使用场景定制握手过程，提供相互认证、匿名发起者或初始握手本身加密的选项。通过利用 Noise 的基于令牌的握手系统，NHP 可以精确控制密钥交换消息的顺序，同时保持身份信息的机密性。

#### 2.1.2 数据加密

共享密钥在握手期间派生出来后，Noise 框架使用对称加密来保护数据。NHP 利用 Noise 的 CipherState 和 SymmetricState 对象，这些是 Noise 状态机的核心组件，用于管理通信会话的加密和解密密钥。

特别地，握手期间派生的共享密钥用于初始化对称加密密钥（k）和随机数（n），用于数据加密。Noise 支持高级加密方案，如 ChaCha20-Poly1305 或 AES-GCM，提供带有附加数据认证加密（AEAD），以维护数据的保密性和完整性。链式密钥（ck）和握手散列（h）用于在会话过程中不断派生新的密钥，增强前向安全性，确保一个密钥的泄露不会危及通信的其他部分。

NHP 通过这些加密属性提供网络数据的加密通道，确保任何被拦截的数据在没有派生密钥的情况下无法被解密。

#### 2.1.3 身份验证

Noise 通过将静态密钥的交换与 Diffie-Hellman 操作相结合来实现身份验证。在 NHP 中，身份验证发生在握手期间，其中静态密钥被加密并通过共享的 DH 操作进行验证，有效地将双方的公钥绑定到派生的会话密钥上。

在握手过程中，Noise 使用诸如 "s"（静态）和 "e"（临时）等令牌来指示正在交换和验证哪些密钥。这种基于令牌的方法使得 NHP 能够根据具体的使用场景选择性地认证单方或双方。例如，Noise 中的 "XX" 模式提供相互认证，而 "NK" 模式允许单方认证的握手，赋予 NHP 在身份验证严格性方面的灵活性。

为了进一步保护身份信息，Noise 可以在握手期间加密静态密钥。NHP 利用这一特性来防止窃听者发现参与者的身份，从而支持零信任模型，确保参与者的身份只对预期的对方可见，而不对第三方泄露。

### 2.2 算法和公式

#### 2.2.1 Diffie-Hellman 密钥交换

Diffie-Hellman（DH）密钥交换用于在两方之间派生共享密钥，每一方生成一个私钥（a 对于 A，b 对于 B）并计算公共密钥：

- A 计算其公钥：A_pub = g^a mod p
- B 计算其公钥：B_pub = g^b mod p

共享密钥 s 通过以下方式计算：

- A 计算：s = B_pub^a mod p
- B 计算：s = A_pub^b mod p

该共享密钥 s 对于双方是相同的，用于派生加密密钥。

#### 2.2.2 对称加密

NHP 使用对称加密确保数据机密性。密钥（k）和随机数（n）用于加密函数。对于带有附加数据认证加密（AEAD），通常使用 ChaCha20-Poly1305 算法，该算法结合了流密码（ChaCha20）和消息验证码（Poly1305）。

- 加密：c = ChaCha20(k, n, plaintext)
- 认证：tag = Poly1305(k, associated data || c)

密文（c）和标签一起传输，确保数据的机密性和完整性。

#### 2.2.3 密钥派生和散列

Noise 使用基于 HMAC 的密钥派生函数（KDF）来派生密钥。HKDF（基于 HMAC 的密钥派生函数）用于从共享密钥（s）生成多个密钥。

HKDF 步骤：

- temp_key = HMAC(chaining_key, input_key_material)
- output1 = HMAC(temp_key, 0x01)
- output2 = HMAC(temp_key, output1 || 0x02)

派生的密钥用于加密和维护链式密钥（ck），以确保前向安全性。

#### 2.2.4 身份认证

身份验证在 NHP 中涉及静态和临时密钥的交换，以验证参与方。静态（s）和临时（e）密钥之间的 Diffie-Hellman 操作产生唯一的共享值，用于验证参与方的身份。

身份验证操作包括：

- ss = DH(s_A, s_B)
- es = DH(e_A, s_B) 或 DH(s_A, e_B)
- ee = DH(e_A, e_B)

这些值通过散列结合起来派生最终的会话密钥，有效地将身份绑定到密钥交换过程中，确保只有预期的参与方能够派生出正确的会话密钥。

### 2.3 结论

Noise 协议框架为 NHP 提供了灵活且强大的加密机制，确保了通信的安全性和参与方身份的验证。通过支持多种握手模式和加密方案，Noise 框架增强了网络的前向安全性和身份保护能力，有效支持了零信任环境下的安全通信需求。结合 Diffie-Hellman 密钥交换、对称加密和基于令牌的身份验证，Noise 框架为 NHP 提供了实现其零信任目标的强大工具。

## 3) 密钥管理与分发

### 3.1 简介

无证书公钥加密（Certificateless Public Key Cryptography，CL-PKC），最初由 Al-Riyami 和 Paterson 在 2003 年提出，提供了一种混合解决方案，在不依赖传统证书机构（CA）的情况下确保强加密保证。本文探讨了 NHP 如何利用 CL-PKC 在不依赖证书的情况下实现高效且安全的密钥管理。

在传统的公钥基础设施（PKI）中，证书机构（CAs）作为可信的第三方，负责签发和管理公钥证书以验证用户密钥的真实性。虽然这种模型有效，但它引入了复杂性和风险，例如对 CA 的依赖以及 CA 受到攻击时的风险。无证书公钥加密旨在通过消除证书的使用来缓解这些问题，同时确保公钥的真实性。

在 CL-PKC 系统中，一个称为密钥生成中心（KGC）的可信第三方负责为用户生成部分私钥。然而，与 CA 不同的是，KGC 无法访问完整的私钥，因此不可能冒充用户。每个用户将 KGC 提供的部分私钥与他们自己的秘密值结合起来，生成完整的私钥和公钥。这种方法减少了对任何单一实体的信任，并提供了额外的安全层。

NHP 零信任协议集成了 CL-PKC 来管理其安全通信框架的密钥分发和验证。下面，我们解释 CL-PKC 的各种机制如何为 NHP 的密钥管理过程做出贡献。

#### 3.1.1. 部分密钥生成

KGC 负责创建系统范围的参数，包括主公私钥对。主私钥由 KGC 保密，而主公钥则分发给所有参与者。当新用户希望加入网络时，KGC 执行以下步骤：

- 生成用户的部分私钥，使用他们的唯一标识符（例如电子邮件或其他身份信息）。这确保每个用户的部分密钥与其身份绑定，提供基于身份的安全性。

##### 3.1.2 用户特定密钥对生成

用户随后选择自己的秘密值，并将其与 KGC 提供的部分私钥结合起来，生成完整的私钥。具体步骤如下：

- 用户选择一个秘密值 (d'\_A)，并使用它来生成一个点 U\_A：

  U\_A = [d'\_A]G

- 用户将 KGC 提供的部分私钥 (t\_A) 与自己的秘密值 (d'\_A) 相结合，计算出完整的私钥 (d\_A)：

  d\_A = (t\_A + d'\_A) mod n

- 用户使用完整的私钥生成相应的公钥 (P\_A)：

  P\_A = W\_A + [l]P\_pub

用户随后选择自己的秘密值，并将其与 KGC 提供的部分私钥结合起来，生成完整的私钥。用户的公钥也相应生成。

这种密钥分发方法确保 KGC 无法单方面确定用户的私钥，从而降低了密钥生成中心被攻破的风险。此外，不需要传统证书意味着用户无需依赖外部证书机构来验证密钥，从而减少了中间人（MITM）攻击的攻击面。

在像 NHP 实施的这种无证书系统中，公钥的真实性通过隐式的方法而不是依赖 CA 签发的证书来验证。具体来说，用户的公钥是使用系统参数、用户标识符和 KGC 的主公钥计算出来的。该计算是确定性的，允许任何一方在无需信任 CA 或存储大量证书数据库的情况下验证公钥的真实性。

通过消除传统证书的需要，NHP 能够简化密钥验证过程，消除证书吊销列表（CRLs）和其他 PKI 复杂性。这种方法不仅减少了通信开销，还通过消除对可信第三方的依赖增强了安全性，因为这些第三方可能成为攻击者的目标。


### 3.2 算法和公式

#### 3.2.1 系统参数生成

密钥生成中心（KGC）负责生成系统参数，这些参数包括椭圆曲线 (E) 定义在有限域 (ℚ_q) 上，基点 (G) 的素数阶 (n)，以及主私钥 (ms)。KGC 计算主公钥 (P_pub) 如下：

P_pub = [ms]G

其中 [ms]G 表示基点 G 与主私钥 ms 的标量乘法。

#### 3.2.2 部分私钥生成

对于每个用户（标识符为 ID_A），KGC 生成部分私钥。首先，KGC 根据用户的标识符和系统参数计算哈希值 (H_A)：

H_A = H(ENTL_A || ID_A || a || b || x_G || y_G || x_P_pub || y_P_pub)

其中 (ENTL_A) 是由标识符派生的长度值，(x_G, y_G) 和 (x_P_pub, y_P_pub) 分别是点 G 和 P_pub 的坐标。

KGC 选择一个随机值 (w ∈ [1, n-1]) 并计算：

W_A = [w]G + U_A

其中 (U_A = [d'_A]G) 是用户使用其自身的秘密值 (d'_A) 生成的点。

部分私钥 (t_A) 计算如下：

t_A = (w + l · ms) mod n

其中 (l) 是根据点 (W_A) 和哈希 (H_A) 计算的值。

#### 3.2.3 用户完整私钥生成

用户通过将部分私钥 (t_A) 与其秘密值 (d'_A) 结合生成完整的私钥：

d_A = (t_A + d'_A) mod n

这确保只有用户自己知道其完整的私钥。

#### 3.2.4 公钥计算

用户的公钥 (P_A) 计算如下：

P_A = W_A + [l]P_pub

任何人可以使用系统参数、用户标识符和 KGC 的公钥验证该公钥。

#### 3.2.5 签名生成与验证

用户对消息 (M) 生成数字签名时，计算哈希 (e) 如下：

e = H(H_A || x_W_A || y_W_A || M)

签名 (r, s) 使用用户私钥 (d_A) 和一个随机值 (k) 生成：

[r]G = (x_1, y_1)
r = x_1 mod n
s = (k^{-1}(e + d_A · r)) mod n

验证签名时，验证者计算 (P_A)，然后检查是否满足：

[r]G = [s]G + [e + r]P_A

如果等式成立，签名即为有效。


### 3.3 结论

NHP 实施的无证书公钥加密为零信任环境中的密钥管理提供了一种强大且高效的方法。通过利用 CL-PKC，NHP 能够缓解传统 PKI 相关的风险，减少对集中可信机构的依赖，并简化密钥分发过程。结果是一个更安全且可扩展的系统，适合在面对不断发展的网络威胁时保护关键网络基础设施。

将无证书加密与 NHP 的零信任原则相结合，使其成为在最小化集中机构引入的风险的同时保护网络资源的理想解决方案。

Copyright © 2024 OpenNHP Open Source Project.

