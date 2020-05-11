![PNG](/asset/img/banner1.png)

<h1 style='border-bottom:2px solid #07823A;color:#07823A;'>*1.6 数据标准</h1>

我们可以出于多种多样的目的以不同的方式格式化数据，制作出各种惊人的数据可视化图表。但是这些数据最终趋向于这几种可识别的类：表格数据，嵌套数据，网络数据，地理数据，原始数据和对象。

### <p style='color:#07823A'>1. 表格数据</p>
- 在电子表格或数据库表中，通常是以行列来表现的表格数据。尽管我们最终都会在D3中创建对象数组，但是以表格格式提取数据通常更高效，更容易。
- 表格数据使用特定字符进行分隔，然后由分隔符确定其格式。我们可以使用CSV（Comma-Separated Values ）文件，它的分隔符可以是逗号、制表符分隔值、分号或管道符号（|）。
- D3提供三种不同的功能来提取表格数据：<font style='color:#e51c23;font-weight:bold;'>d3.csv（），d3.tsv（）和d3.dsv（）。</font> 它们之间的唯一区别是：
     * d3.csv( ) 是由逗号分隔的文件构建的，
     * d3.tsv() 是由制表符分隔的文件构建，
     * d3.dsv() 则允许我们声明分隔符。

#### <label style='background:#CF57FF;color:#fff;font-size:16px;display:inline-block;line-height:22px;padding:3px 20px 3px 8px;'>*example: 使用不同的格式分隔数据</label>

<table>
    <thead>
        <tr style="background:#03a9f4; color:#fff;">
            <th>Name,Age,Salary</th>
            <th>Name Age Salary</th>
            <th>Name|Age|Salary</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1.1 d3.csv</td>
            <td>1.2 d3.tsv </td>
            <td>1.3 d3.dsv </td>
        </tr>
        <tr>
            <td>Sal,34,5000</td>
            <td>Sal 34 5000</td>
            <td>Sal|34|5000</td>

        </tr>
        <tr>
            <td>Nan,22,3000</td>
            <td>Nan 22 3000</td>
            <td>Nan|22|3000</td>

        </tr>
    </tbody>
</table>

### <p style='color:#07823A'>2. 嵌套数据（CH6）</p>
嵌套数据，将对象作为对象的子对象递归存在，是很常见的。 D3中许多最直观的布局都基于嵌套数据，这些数据可以表示为树，也可以打包成圆圈或盒形。

### <p style='color:#07823A'>3. 网络数据（CH7）</p>
网络无处不在。 无论是社交网络流，运输网络还是流程图的原始输出，网络都是一种了解复杂系统的有效方法。 网络通常表示为节点链接图。 与地理数据一样，网络数据也有许多标准，但是本文仅关注两种形式：节点/边缘列表和连接的数组。

### <p style='color:#07823A'>4. 地理数据（CH8）</p>
地理数据将位置称为点或形状，并用于创建当今在网络上看到的各种在线地图。 Web映射的惊人普及意味着我们可以访问任何项目的大量可公开访问的地理数据。 地理数据有一些标准，但是本书的重点是两个：GeoJSON和Topo-JSON标准。 尽管地理数据可能以多种形式出现，但诸如Quantum GIS之类的现成的地理信息系统（GIS）工具使开发人员可以将其转换为GIS格式，以便随时交付给网络。

### <p style='color:#07823A'>5. 原始数据（CH2）</p>
正如我们将在第2章中看到的那样，一切都是数据，包括图像或文本块。 尽管信息可视化通常使用颜色和大小编码的形状来表示数据，但是有时在D3中表示数据的最佳方法是使用线性叙述文本，图像或视频。 如果我们为需要理解复杂系统的读者开发应用程序，但是如果认为对文本或图像的操作与以数字或分类数据表示为形状有所不同，那么就会降低沟通能力。 在D3中可以处理文本和图像时使用的布局和格式，通常与较旧的网络发布模式相关联，我们将在整本书中进行介绍。

### <p style='color:#07823A'>6. 对象</p>
D3中使用两种类型的数据点：文字和对象：
- 文字很简单，例如Apple或beer这样的字符串文字或64273或5.44这样的数字文字。
- JavaScript对象或等效的JSON并不是那么简单，但是如果您打算进行复杂的数据可视化，则需要了解这一点。
    - 可以使用d3.select（）语法将对象存储在数组中并与元素关联。
    - 对象可以使用for循环通过类似的数组进行迭代。
    - 访问键的另一种方法是使用Object.keys（person），然后遍历该数组。
    - 如果数据以JSON格式存储，则可以使用d3.json（）导入数据。请注意，每当使用d3.csv（）时，D3都会将数据作为JSON对象数组导入。

#### <label style='background:#CF57FF;color:#fff;font-size:16px;display:inline-block;line-height:22px;padding:3px 20px 3px 8px;'>*example: 代码示例</label>
````
let person = {
                name: "Charlie",
                age: 55,
                employed: true,
                childrenNames:["Ruth", "Charlie Jr."]
             };
//循环中的x表示人员对象中的每个属性
for (x in person) {console.log(x); console.log(person[x]);}
````

### <p style='color:#07823A'>7. D3中表示的Infoviz标准</p>
#### <font style='color:#03a9f4'>7.1 信息可视化</font>
- 信息可视化（Information visualization: infoviz）从未像今天这样流行。 丰富的地图，图表以及系统和数据集的复杂表示形式存在于工作场、娱乐和日常生活中。完善的规则规定了对来自不同系统的不同类型数据使用哪种图表和方法。
- 大多数开发人员都希望创建支持实际问题的可视化表示形式。 因为D3是在这种成熟的信息可视化环境中开发的，所以它包含许多帮助程序功能，使开发人员不必担心界面和设计，而可以担心颜色和轴。
- 要正确部署信息可视化，需要知道该做什么和不该做什么。一些有用的方法可以使我们了解基础知识：
    * The Visual Display of Quantitative Information Envisioning Information, Edward Tufte
    * Designing for Information, Isabel Meirelles
    * Pattern Recognition, Christian Swinehart
    * Visualization Analysis and Design, Tamara Munzner
- 视觉上更复杂的数据显示方法往往会激发更多的兴奋感，但也会导致观众看到他们想要看到的内容，或者将注意力集中在图形的美学而非数据上。

#### <font style='color:#03a9f4'>7.1 交互式和动态可视化（CH9）</font>
 使用D3，您不仅可以进行静态和可视化，还可以进行交互式和动态的可视化。 但是，这种增加的复杂性要求对界面设计和用户体验的学习原理进行学习。 我们将在第9章中对此进行更详细的介绍
