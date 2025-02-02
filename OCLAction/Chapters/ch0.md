# 0. 关于本书

OpenCL 是一个复杂的主题。为了编写最简单的应用程序，开发者需要理解主机编程、设备编程以及在主机和设备之间传输数据的机制。本书的目标是展示如何完成这些任务，并将它们应用于实际的应用程序中。

本书采用教程式的格式。每个新概念后面都会有示例代码，展示如何在应用程序中使用理论。前期的应用程序非常基础，有些只是获取设备和数据结构的信息。但随着书的深入，代码会变得更加复杂，并更充分地利用主机和目标设备。在后期章节中，重点将从学习 OpenCL 的工作原理转向在高速处理大量数据中应用 OpenCL。

## 0.1 读者对象

在撰写本书时，我假设读者从未听说过 OpenCL，也不了解分布式计算或高性能计算。我尽力将诸如任务并行和 SIMD（单指令多数据）开发等概念以最简单、直接的方式呈现。

但是，由于 OpenCL API 基于 C 语言，本书假设读者已经熟悉 C 语言的基础知识。读者应对指针、数组和内存访问函数（如 malloc 和 free）非常熟悉。了解常用数学库中的 C 函数也有帮助，因为大多数内核函数有类似的名称和用法。

OpenCL 应用程序可以在许多不同类型的设备上运行，但它的一个主要优势是可以用于编程图形处理单元 (GPU)。因此，为了充分利用本书，最好为计算机配备图形卡，或使用像 AMD 的 Fusion 这样的混合 CPU-GPU 设备。

## 0.2 路线图

本书分为三部分。第一部分（第 1-10 章）重点介绍 OpenCL 语言及其功能。第二部分（第 11-14 章）展示了如何使用 OpenCL 执行高性能计算中常见的大规模任务。最后一部分（第 15 和 16 章）展示了如何使用 OpenCL 加速 OpenGL 应用程序。

第一部分的章节结构服务于从未编写过 OpenCL 代码的程序员需求。第 1 章介绍了 OpenCL 的基本概念，包括其来源及其工作原理。第 2 和 3 章解释如何编写在主机上运行的应用程序，第 4 和 5 章展示如何编写在符合标准的设备上运行的内核。第 6 和 7 章探讨涉及主机编程和内核编码的高级主题，具体来说，第 6 章介绍图像处理，第 7 章讨论事件处理和同步的重要话题。

第 8 和 9 章讨论了第 2 到 5 章中提出的概念，但使用 C 语言以外的语言。第 8 章讨论了 C++ 的主机/内核编码，第 9 章解释了如何用 Java 和 Python 构建 OpenCL 应用程序。如果没有编程语言的限制，我建议使用这些章节中讨论的工具集之一。

第 10 章是第 1 部分和第 2 部分之间的桥梁。它通过实现一个简单的归约算法（将一百万个数据点相加）来展示如何充分利用 OpenCL 的并行性，还提供了编写实用 OpenCL 应用程序的有用指南。

第 11 到 14 章深入探讨了 OpenCL 的重度使用，通常处理数百万个数据点的应用程序。第 11 章讨论了 MapReduce 的实现和两种排序算法：位onic 排序和基数排序。第 12 章涉及稠密矩阵的操作，第 13 章探讨稀疏矩阵的操作。第 14 章解释了如何使用 OpenCL 实现快速傅里叶变换 (FFT)。

第 15 和 16 章是我个人最喜欢的章节。OpenCL 的一个强大优势是可以加速三维渲染，这是游戏开发和科学可视化的核心主题。第 15 章介绍了 OpenCL-OpenGL 互操作性，并展示了这两种工具集如何共享与顶点属性相关的数据。第 16 章进一步探讨如何通过 OpenCL 加速 OpenGL 纹理处理。这些章节需要对 OpenGL 3.3 和着色器开发有一定的了解，这两个主题在附录 B 中有探讨。

## 0.3 获取和编译示例代码

最终，代码才是最重要的。本书包含 60 多个 OpenCL 应用程序的工作代码，您可以从出版商的网站下载源代码（网址为 www.manning.com/OpenCLinAction 或 www.manning.com/scarpino2/）。

下载网站提供了一个链接，指向一个包含代码的压缩文件，这些代码是为使用基于 GNU 的构建工具编译而设计的。该压缩文件为本书的每一章或附录提供了一个文件夹，每个顶层文件夹都有相应的示例项目子文件夹。例如，在 Ch5/shuffle_test 目录中，您可以找到第 5 章 shuffle_test 项目的源代码。

关于依赖项，每个项目都要求开发系统中有 OpenCL 库（在 Windows 上是 OpenCL.lib，在类 Unix 系统上是 libOpenCL.so）。附录 A 讨论了如何通过安装适当的软件开发工具包（SDK）来获取该库。

此外，第 6 和 16 章讨论了图像处理，这些章节中的源代码使用了开源的 PNG 库。第 6 章解释了如何为不同系统获取该库。附录 B 以及第 15 和 16 章都需要访问 OpenGL，附录 B 说明了如何获取并安装该工具集。

## 0.4 代码规范

虽然这可能听起来很懒，但我更喜欢将已工作的代码复制粘贴到我的应用程序中，而不是从头编写代码。这不仅节省时间，还减少了因输入错误而产生 bug 的可能性。本书中的所有代码都是公有领域的，因此您可以自由下载并将其部分内容复制粘贴到您的应用程序中。但在此之前，了解我使用的一些规范是很有帮助的：

- 主机数据结构以其数据类型命名。也就是说，每个 `cl_platform_id` 结构体称为 `platform`，每个 `cl_device_id` 结构体称为 `device`，每个 `cl_context` 结构体称为 `context`，等等；
- 在主机应用程序中，`main` 函数调用两个函数：`create_device` 返回一个 `cl_device`，而 `build_program` 创建并编译一个 `cl_program`。注意，`create_device` 会查找与第一个可用平台关联的 GPU。如果找不到 GPU，它会查找第一个符合条件的 CPU；
- 主机应用程序使用在源文件开头声明的宏来识别程序文件和内核函数。具体来说，`PROGRAM_FILE` 宏标识程序文件，`KERNEL_FUNC` 宏标识内核函数；
- 我的所有程序文件都以 `.cl` 为后缀。如果程序文件中只包含一个内核函数，则该函数与文件同名；
- 对于 GNU 代码，每个 `makefile` 都假定库和头文件位于环境变量标识的位置。具体来说，`makefile` 会在 AMD 平台上搜索 `AMDAPPSDKROOT`，在 Nvidia 平台上搜索 `CUDA`；

## 0.5 作者在线

没人是完美的。如果我没有清楚地传达我的主题，或者（惊讶）犯了错误，请通过 Manning 的 "Author Online" 系统留言。您可以访问 www.manning.com/OpenCLinAction，然后点击 "Author Online" 链接找到本书的在线论坛。

简单的问题和疑虑通常会得到快速回复。相反，如果您对我为 bitonic 排序算法实现的第 402 行感到不满，可能需要一些时间我才能回复。关于 OpenCL 的一般问题我很乐意讨论，但如果您寻求复杂而具体的帮助，比如调试自定义的 FFT，我可能会建议您寻求专业顾问。

## 0.6 封面插图简介

《OpenCL in Action》封面上的人物图像标题为 “Kranjac”，即斯洛文尼亚阿尔卑斯山克拉尼奥拉地区的居民。这幅插图取自 Balthasar Hacquet 的著作《西南和东部温达人、伊利里亚人和斯拉夫人的图像与描述》的最近再版，该书由克罗地亚斯普利特的民族志博物馆于 2008 年出版。

Hacquet（1739–1815）是一位奥地利医生和科学家，花费了多年时间研究朱利安阿尔卑斯山的植物学、地质学和民族学。朱利安阿尔卑斯山是从意大利东北部延伸至斯洛文尼亚的山脉，以尤利乌斯·凯撒命名。Hacquet 的许多科学论文和书籍都附有手绘插图。

Hacquet 著作中丰富多样的插图生动展现了 200 年前东阿尔卑斯地区的独特性和个性。在那个时代，相隔几英里的两个村庄的服饰风格能明显区分彼此，社会阶层或职业的成员也可以通过他们的穿着轻松辨别。然而，如今的着装风格已经改变，那个时代各地区的丰富多样性已逐渐消失。如今，已很难区分一个大陆的居民和另一个大陆的居民，今日斯洛文尼亚阿尔卑斯山那些风景如画的城镇和村庄的居民也与斯洛文尼亚其他地区或欧洲其他地方的居民不再有显著区别。

我们在 Manning 出版社通过这样的插图，重现了两个世纪前的服饰风格，庆祝计算机行业的创造力、创新精神和乐趣。