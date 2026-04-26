# cad-generation-paper-list
标注的时间是首发日期

## CAD generation in B-rep format
1. (SIGGRAPH 2024) BrepGen: A B-rep Generative Diffusion Model with Structured Latent Geometry
[[paper]](https://arxiv.org/abs/2512.03018) 2024.11 2个VAE+4个LDM，面和边由不同模块负责，生成出的采样点由opencascade库重建 主要为无条件生成 一个数据集有文本类别选择（家具数据集）
[[code]](https://github.com/samxuxiang/BrepGen)

*2. (CVPR 2025) DTGBrepGen: A Novel B-rep Generative Model through Decoupling Topology and Geometry
[[paper]](https://arxiv.org/pdf/2503.13110) 2025.3
[[code]](https://github.com/jinli99/DTGBrepGen)
效果：

*3. BrepGPT: Autoregressive B-rep Generation with Voronoi Half-Patch
[[paper]](https://arxiv.org/pdf/2511.22171)) 2025.11
[[code]](https://github.com/BunnySoCrazy/BrepGPT)

*4. (SIGGRAPH Asia 2025) AutoBrep: Autoregressive B-Rep Generation with Unified Topology and Geometry 
[[paper]](https://arxiv.org/abs/2512.03018) 2025.12 自回归生成 
[[code]](https://github.com/AutodeskAILab/AutoBrep?tab=readme-ov-file) 

*5. 【BrepARG】AutoRegressive Generation with B-rep Holistic Token Sequence Representation
[[paper]](https://arxiv.org/pdf/2601.16771) 2026.1
[[code]](https://github.com/123qiang06/BrepARG)
Validity算法是success rate * watertight rate
watertight rate检查
STEP 能读入
只有一个 solid
wire 顺序正常
wire 无自交
shell 没有 bad edges
BRep 没有 free edges，也就是 watertight/closed
 
6. Flatten The Complex: Joint B-Rep Generation via Compositional 𝑘-Cell Particles
7. BrepDiff


## 其他格式
1. (SIGGRAPH Asia 2025) Img2CAD: Reverse Engineering 3D CAD Models from Images
[[paper]](https://arxiv.org/abs/2408.01437) 2024.6 
[[code]](https://github.com/qq456cvb/Img2CAD)  

   
# assembly
ArtiCAD
[[paper]](https://arxiv.org/abs/2408.01437) 2024.6 
[[code]](https://github.com/qq456cvb/Img2CAD)  
## related work 
Leo AI 没有生成零件的过程，输入文本需求（生成一个如何如何的装配体），用ai先查找库内零件（没有就联网搜索），然后给装配方案，装配过程也需要人工参与（参与多少？演示视频直接跳过了人工步骤，这个工作看起来还很初级）
## 工作
无训练，agent/由文本生成多种类型的assembly/支持多种关节类型：Fixed（固定）、Revolute（旋转）、Slider（滑动）、Cylindrical（柱面）、Ball（球形）。（显得给assembly也分了不同类型很没意义）
URDF文件，直接用于SAPIEN、Isaac Sim等仿真环境。导出的 URDF 保留了预期的关节结构、轴方向和运动限制。（感觉是机器人领域？）
导出完整的关节装配体是 STL 格式文件。

## 关键
设计阶段定义Connector（"桌面底部4个角各有一个连接点"）→ 生成阶段将Connector精确放置到对应几何位置（如圆柱顶面正中心）→ 装配阶段确定性对齐。

## 数据集
ArtiCAD-Bench 数据集用于评价生成效果
两个子集120 个装配任务。第一个子集包括 90 个多样化的真实世界设计（50 个运动学装配，40 个静态装配），涵盖家具、玩具和家用电器。第二个子集包含 30 个从 Fusion 360 数据集中精选的工业装配，包含 2 到 6 个部件。

## 可参考的
1. 多阶段生成的这种思路，同时有对关节的约束，每一个阶段的agent会对关节信息进一步精确，不至于说生成部件和部件装配太打架
2. 可以参考的实验和评估指标， 在ArtiCAD-Bench, CADPrompt, and ACD三个数据集上做的，评估是对模型多角度渲染（包括articulated models的关节运动关键帧，为什么他不提工业assembly？joinable数据集明明都是这种），之后让三个VLM打分（感觉这里不是很透明，没有测评传统生成任务里的有效率，成功率=有效率？成功率竟然达到了百分之百，虽然可以理解这种代码生成的模型没有什么复杂度，成功率会高）
3. 生成.FCstd这种格式倒是挺便利的，值得借鉴，如果按照我考虑的硬要生成b-rep(也就是step)，就丢失了关节信息，两个部件能装上的难度也很大，兜了个大圈子。
   但是代码生成这块，生成的模型就比较简单了（因为代码是按照先画草图再拉伸的逻辑写的，也用那种基元拼接的函数，像是那种自由曲面、流体外壳什么，依然也是做不了，这是代码生成这种方法无法突破的），估计joinable数据集那种的生成不了，而且代码生成开源项目之前只找到一个，还通通是用大模型做的。

# 一些和生成不怎么相关的论文
1. 【neuralCAD-Edit】 neuralCAD-Edit: An Expert Benchmark for Multimodal-Instructed 3D CAD Model Editing
[[paper]](https://arxiv.org/abs/2604.16170) 2026.4
[[code]](https://github.com/AutodeskAILab/neuralCAD-Edit)
关于CAD模型编辑的工作，不是生成
先记录编辑要求，然后让人类专家和AI一起编辑，AI获得的是原始step文件，用CadQuery Python 脚本编辑（这个肯定不会准啊，我觉得光是写CadQuery Python 脚本复刻这些step都会有问题）
其实主要就是提供了数据集和benchmark，可以比较专业的评测AI的编辑能力。
数据集的全面之处在于编辑指令多模态：视频 / 语音 / 手绘 / 鼠标交互
评测指标
（1）基础：
Chamfer 距离：点云误差，越小越好。
体素 IoU：3D 重叠率，越高越好。
DINOv2 相似度：渲染图视觉特征，越高越好。
Validity：有效模型占比。（实际是判断了有没有渲染出图片，没有关是不是水密）
（2）/（3）：人类评估，就是打打分，打分具体要求不赘述，输入都是下面三部分
请求；
输出模型的六个正交视图：top、bottom、front、back、left、right；
一个 isometric view（等轴测投影）。

## 想做好 neuralCAD-Edit，需要同时具备：
（1）多模态理解能力
看懂视频、语音、绘图、鼠标交互这些编辑要求。
（2）代码能力
因为 AI 通过写 CadQuery 脚本来编辑模型。
（3）3D 空间推理和几何操作能力
能理解 3D 相对位置、方向、结构关系，并准确修改模型。

2. 【STEP-Parts】: Geometric Partitioning of Boundary Representations for Large-Scale CAD Processing
[[paper]](https://arxiv.org/abs/2604.14927) 2026.4 
做B-rep的实例分割的，提供工具链和数据集，说是发布了，哪儿也没找到

## 重点
并非是什么功能性部件或者外观上部件的分割，而是分割成了一个个几何区域，取决于
B-Rep 面之间是否相邻；
面的解析曲面类型是否相同；
相邻面在公共边处是否近似切平滑；

## related works
【PartGen】Part-level 3D generation and reconstruction with multi-view diffusion models
【PartCrafter】: Structured 3D Mesh Generation via √
【Assembler】: Scalable 3D Part Assembly via Anchor Point Diffusion

