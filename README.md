# 晨光计划-全景多层次场景图生成数据集

项目负责人：励雪巍，项目号：24CGA73

<p align="center">
  <img src="dataset-wordmark.png" width="640" alt="数据集标识">
</p>

**面向全景分割与场景图生成的高分辨率ERP图像数据集。**

本数据集将像素级场景标注与实体间语义关系相连接。在完整360°全景视野中，全景分割掩码描述场景实体所占据的可见区域，“主体—谓词—客体”三元组则描述实体之间的语义联系。两部分标注通过共同的实体身份建立对应，将图像区域、实体节点和关系边组织在同一套场景表示中。

这一组织方式为全景场景解析与关系理解研究提供数据基础。全景分割关注场景中有哪些实体及其可见区域，场景图生成在此基础上进一步描述实体之间的关系。本数据集围绕同一组全景图像，将这两种相互补充的场景描述连接起来。

本数据集包含 **342张全景图**，每张分辨率为 **11968 × 5984**，具有 **10,730个原始实体节点和7,017条关系三元组**。对象采用COCO Panoptic的133类体系，其中83类实际出现；关系采用21类谓词。图像内容、实体掩码、对象类别与语义谓词共同描述实体的空间范围及其在全景场景中的联系。

## 数据特点

- **高分辨率360°场景。** 每张图像约含7162万像素，覆盖街道、校园、公园等环境，保留丰富的场景细节。全景视野将独立实体置于周围环境之中，使局部视觉信息与整个视野中的场景上下文能够在同一张图像中呈现。
- **全景分割与场景图关联。** thing实例与stuff区域共同构成实体节点，关系三元组连接同一组实体。场景表示既包含独立对象，也包含较大范围的环境区域，使不同类型的分割实体能够通过语义关系建立联系。
- **多谓词关系表达。** 同一有序对象对支持多个谓词，描述实体间的语义联系。三元组的主体与客体对应分割标注中的实体身份，使关系图中的节点能够关联到具体图像区域。
- **跨接缝实体身份。** 通过源标注分组，将ERP左右边界同一实例的可见区域关联为统一实体。即使这些区域在矩形图像中彼此分离，也可在场景表示中作为同一节点参与关系表达。
- **人工标注与复核。** 标注人员经过统一培训，全部标注经过复核，检查掩码、关系端点、重复三元组及分割与关系的对应情况。复核同时关注分割标注本身，以及分割实体与关系端点之间的联系。

## 数据统计

图像数量与分辨率描述原始视觉数据，节点数与三元组数描述标注场景图的规模，类别数则说明对象和关系所采用的词表范围。

| 项目 | 数量 |
| --- | ---: |
| ERP全景图 | 342张 |
| 每张图像分辨率 | 11968 × 5984 |
| 每张图像像素数 | 约7162万 |
| 对象预定义类别 | 133类 |
| 实际出现对象类别 | 83类 |
| 原始实体节点 | 10,730个 |
| thing节点 | 7,916个 |
| stuff节点 | 2,814个 |
| 关系三元组 | 7,017条 |
| 谓词类别 | 21类 |
| 每图平均节点 | 31.37个 |
| 每图平均关系 | 20.52条 |

## 实体与关系

实体通过全景分割掩码定位，并保留对象类别与稳定身份。“主体—谓词—客体”三元组描述实体之间的语义联系，统一采用21类谓词。

在此基础上，数据集引入实体组合与组合关系，将场景表达从单个实体之间的联系扩展至多个实体组成的整体。关系的主体和客体均可为单个实体或实体组合，从而支持实体之间、实体与组合之间以及组合之间的关系表达。场景图中的组合关系体现了数据集的多层次结构，使个体层面的联系与实体整体层面的联系能够在同一张场景图中共同呈现。组合成员通过稳定ID关联原始实体，保留各成员的独立身份及其对应的分割掩码。

## 采集与标注

图像由数据集创建者自行拍摄，以ERP全景图形式组织，分辨率统一为11968 × 5984。采集内容涵盖街道、校园、公园等环境。在图像整理过程中，排除模糊、重复和拼接错误的图片，形成用于构建数据集的原始图像集合。

全景分割掩码由人工在CVAT中标注。可辨认区域赋予对象类别，无法辨认区域标为ignore。thing标注区分独立对象实例，同图同类stuff区域汇总为一个节点；由此，每个实体节点将语义类别与其对应的一个或多个可见图像区域连接起来。

跨ERP左右接缝的同一thing实例通过共同源分组，合并为一个实体和一个segment ID。当同一对象的可见部分位于矩形图像两端时，这种组织方式保留其统一身份，使分割表示和后续关系端点共同指向同一个场景实体。

关系标注平台通过稳定实体身份关联掩码区域与关系端点。标注以全景分割得到的实体为基础，选择主体、谓词与客体，构成对应的关系三元组。标注支持thing–thing、thing–stuff和stuff–stuff之间的关系，同一有序实体对也可关联多个谓词，从而在已有实体区域之上表达语义联系。

空间关系依据场景中的实际关系判断，包括位于ERP图像两端但现实相邻的实体。矩形图像的边界不被直接视为现实场景的分隔，关系解释仍以所描绘的场景为依据。通过共同的实体身份，关系中的主体和客体始终关联相应的分割实体。

全部标注经过复核。其中，掩码复核关注全景分割区域，端点有效性检查核对主体和客体是否指向有效实体，重复三元组检查识别重复记录。掩码与关系对齐检查进一步核对关系端点是否对应预期的分割实体，使两部分标注在同一复核流程中得到检查。

## 可视化展示

以下示例展示相互对应的原始ERP全景图、全景分割可视化及场景图。每行三张图来自同一张全景图像。

<table width="100%">
  <thead>
    <tr>
      <th width="33%" align="center">01 · 原始全景图</th>
      <th width="33%" align="center">02 · 全景分割</th>
      <th width="33%" align="center">03 · 场景图</th>
    </tr>
  </thead>
  <tbody>
    <tr><td colspan="3" align="left"><sub>IMG_20260417_161924_00_323</sub></td></tr>
    <tr>
      <td width="33%" align="center" valign="middle"><a href="sample_323_original.jpg"><img src="sample_323_original.jpg" width="100%" alt="原始全景图 — IMG_20260417_161924_00_323"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_323_segmentation.png"><img src="sample_323_segmentation.png" width="100%" alt="全景分割 — IMG_20260417_161924_00_323"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_323_scene_graph.png"><img src="sample_323_scene_graph.png" width="100%" alt="场景图 — IMG_20260417_161924_00_323"></a></td>
    </tr>
    <tr><td colspan="3" align="left"><sub>IMG_20260505_134834_00_422</sub></td></tr>
    <tr>
      <td width="33%" align="center" valign="middle"><a href="sample_422_original.jpg"><img src="sample_422_original.jpg" width="100%" alt="原始全景图 — IMG_20260505_134834_00_422"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_422_segmentation.png"><img src="sample_422_segmentation.png" width="100%" alt="全景分割 — IMG_20260505_134834_00_422"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_422_scene_graph.png"><img src="sample_422_scene_graph.png" width="100%" alt="场景图 — IMG_20260505_134834_00_422"></a></td>
    </tr>
    <tr><td colspan="3" align="left"><sub>IMG_20260505_135147_00_426</sub></td></tr>
    <tr>
      <td width="33%" align="center" valign="middle"><a href="sample_426_original.jpg"><img src="sample_426_original.jpg" width="100%" alt="原始全景图 — IMG_20260505_135147_00_426"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_426_segmentation.png"><img src="sample_426_segmentation.png" width="100%" alt="全景分割 — IMG_20260505_135147_00_426"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_426_scene_graph.png"><img src="sample_426_scene_graph.png" width="100%" alt="场景图 — IMG_20260505_135147_00_426"></a></td>
    </tr>
    <tr><td colspan="3" align="left"><sub>IMG_20260505_142530_00_427</sub></td></tr>
    <tr>
      <td width="33%" align="center" valign="middle"><a href="sample_427_original.jpg"><img src="sample_427_original.jpg" width="100%" alt="原始全景图 — IMG_20260505_142530_00_427"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_427_segmentation.png"><img src="sample_427_segmentation.png" width="100%" alt="全景分割 — IMG_20260505_142530_00_427"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_427_scene_graph.png"><img src="sample_427_scene_graph.png" width="100%" alt="场景图 — IMG_20260505_142530_00_427"></a></td>
    </tr>
    <tr><td colspan="3" align="left"><sub>IMG_20260508_152303_00_473</sub></td></tr>
    <tr>
      <td width="33%" align="center" valign="middle"><a href="sample_473_original.jpg"><img src="sample_473_original.jpg" width="100%" alt="原始全景图 — IMG_20260508_152303_00_473"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_473_segmentation.png"><img src="sample_473_segmentation.png" width="100%" alt="全景分割 — IMG_20260508_152303_00_473"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_473_scene_graph.png"><img src="sample_473_scene_graph.png" width="100%" alt="场景图 — IMG_20260508_152303_00_473"></a></td>
    </tr>
    <tr><td colspan="3" align="left"><sub>IMG_20260510_133848_00_498</sub></td></tr>
    <tr>
      <td width="33%" align="center" valign="middle"><a href="sample_498_original.jpg"><img src="sample_498_original.jpg" width="100%" alt="原始全景图 — IMG_20260510_133848_00_498"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_498_segmentation.png"><img src="sample_498_segmentation.png" width="100%" alt="全景分割 — IMG_20260510_133848_00_498"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_498_scene_graph.png"><img src="sample_498_scene_graph.png" width="100%" alt="场景图 — IMG_20260510_133848_00_498"></a></td>
    </tr>
    <tr><td colspan="3" align="left"><sub>IMG_20260511_172245_00_618</sub></td></tr>
    <tr>
      <td width="33%" align="center" valign="middle"><a href="sample_618_original.jpg"><img src="sample_618_original.jpg" width="100%" alt="原始全景图 — IMG_20260511_172245_00_618"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_618_segmentation.png"><img src="sample_618_segmentation.png" width="100%" alt="全景分割 — IMG_20260511_172245_00_618"></a></td>
      <td width="33%" align="center" valign="middle"><a href="sample_618_scene_graph.png"><img src="sample_618_scene_graph.png" width="100%" alt="场景图 — IMG_20260511_172245_00_618"></a></td>
    </tr>
  </tbody>
</table>

<p align="center"><sub>点击任一图片可单独查看。</sub></p>

全景分割可视化通过不同颜色展示标注区域，呈现实体的可见范围与边界，并与同一行的原始全景图相对应。

场景图中的小圆圈表示单个实体，圈内标注对象类别与实体编号；大圆圈表示由多个实体组成的组合节点，内部的小圆圈展示其成员。箭头由主体指向客体，箭头旁的文字表示关系谓词。连接大圆圈边界的箭头表示面向实体组合整体的关系，与单个实体之间的关系共同体现数据集的多层次结构。

同一实体在场景图中重复出现时，保持相同的名称、编号和颜色，表示其具有相同的实体身份。实体组合保留各成员的独立身份，组合关系描述以这些成员整体为端点的语义联系。

## 数据下载

原始图像集合包含342张完整ERP全景图，分辨率统一为11968 × 5984。图像文件名单见[image_files_342.txt](image_files_342.txt)。原始图像下载链接如下。

| 内容 | 下载链接 | 提取码 |
| --- | --- | --- |
| 原始ERP图像，共342张 | [阿里云盘下载](https://www.alipan.com/s/jyeM4QkHUDi) | `k51t` |

## 使用许可

本数据集图像与标注采用 **[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)**，详见[许可声明](LICENSE)。使用时须适当署名、提供许可链接并说明修改。

## 联系

数据使用与反馈请联系 **xueweili@sdju.edu.cn**，也可通过GitHub Issues反馈。
