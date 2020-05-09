![PNG](/asset/img/banner1.png)

<h1 style='border-bottom:2px solid #07823A;color:#07823A;'>*1.5 D3&SVG</h1>

### <p style='color:#07823A'>1. 基本定义</p>

可缩放矢量图形（Scalable Vector Graphics，SVG），是一种用于描述基于二维的矢量图形的，基于 XML 的标记语言。SVG提供了一些元素，用于定义圆形(circle)、矩形(rect)、简单或复杂的曲线(line)，以及其他形状。一个简单的SVG文档由&lt;svg>根元素和基本的形状元素构成。另外还有一个g元素，它用来把若干个基本形状编成一个组。

### <p style='color:#07823A'>2. D3中的基本元素</p>

<table>
    <thead style="color:#fff;font-weight:bold;">
        <tr style="background:#03a9f4;">
            <th>元素</th>
            <th>描述</th>
            <th>专有属性</th>
            <th>属性含义</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="10" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;SVG&gt;</td>
            <td rowspan="10">画布容器,左上角为0,0</td>
            <td>version</td>
            <td>指明 SVG 文档遵循规范</td>
        </tr>
        <tr>
            <td>baseProfile</td>
            <td>描述了作者认为正确渲染内容所需要的最小的 SVG 语言概述：</br>
            none-代表了最小的 SVG 语言配置</br>
            full-代表一个正常的概述，适用于 PC</br>
            basic-代表一个轻量级的概述，适用于 PDA</br>
            tiny-代表更轻量的概述，适用于手机</br>
            </td>
        </tr>
        <tr>
            <td>x</td>
            <td>x轴坐标</td>
        </tr>
        <tr>
            <td>y</td>
            <td>y轴坐标</td>
        </tr>
        <tr>
            <td>width</td>
            <td>宽度</td>
        </tr>
        <tr>
            <td>height</td>
            <td>高度</td>
        </tr>
        <tr>
            <td>preserveAspectRatio</td>
            <td>是否强制进行统一缩放。通常我们使用viewBox属性时, 希望图形拉伸占据整个视口。在其他情况下，为了保持图形的长宽比，必须使用统一的缩放比例</td>
        </tr>
        <tr>
            <td>contentScriptType</td>
            <td>sdfa</td>
        </tr>
        <tr>
            <td>contentStyleType</td>
            <td>sdfa</td>
        </tr>
        <tr>
            <td>viewBox</td>
            <td>包含4个参数的列表 min-x, min-y, width and height;指定一个给定的一组图形伸展以适应特定的容器元素</td>
        </tr>
        <tr>
            <td rowspan="3" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;CIRCLE&gt;</td>
            <td rowspan="3">圆,基于一个圆心和一个半径</td>
            <td>cx</td>
            <td>定义一个中心点的 x 轴坐标</td>
        </tr>
        <tr>
            <td>cy</td>
            <td>定义一个中心点的 y 轴坐标</td>
        </tr>
        <tr>
            <td>r</td>
            <td>定义圆的半径</td>
        </tr>
        <tr>
            <td rowspan="2" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;RECT&gt;</td>
            <td rowspan="2">矩形</td>
            <td>rx</td>
            <td>用于定义x轴向的圆角半径尺寸</td>
        </tr>
        <tr>
            <td>ry</td>
            <td>用于定义y轴向的圆角半径尺寸</td>
        </tr>
        <tr>
            <td rowspan="4" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;LINE&gt;</td>
            <td rowspan="4">一条连接两个点的线</td>
            <td>x1</td>
            <td>用于绘制需要多个坐标的SVG元素的第一个x坐标</td>
        </tr>
        <tr>
            <td>x2</td>
            <td>用于绘制需要多个坐标的SVG元素的第二个x坐标</td>
        </tr>
        <tr>
            <td>y1</td>
            <td>用于绘制需要多个坐标的SVG元素的第一个y坐标</td>
        </tr>
        <tr>
            <td>y2</td>
            <td>用于绘制需要多个坐标的SVG元素的第二个y坐标</td>
        </tr>
        <tr>
            <td rowspan="1" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;POLYGON&gt;</td>
            <td>由一组首尾相连的直线线段构成的闭合多边形形状。最后一点连接到第一点</td>
            <td>points</td>
            <td>用来画一个SVGElement("polygon")元素的点的数列。</br> 每个点用用户坐标系统中的一个X坐标和Y坐标定义。用逗号分开每个点的X和Y坐标标记是一个常用实践（但是并非必要），使用空间标注每个点
            </td>
        </tr>
        <tr>
            <td rowspan="8" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;TEXT&gt;</td>
            <td rowspan="8">由文字组成的图形</td>
            <td>x</td>
            <td>标识了一个x轴坐标</td>
        </tr>
        <tr>
            <td>y</td>
            <td>标识了一个y轴坐标</td>
        </tr>
        <tr>
            <td>dx</td>
            <td>一个元素或其内容在x轴方向上的偏移</td>
        </tr>
        <tr>
            <td>dy</td>
            <td>一个元素或其内容在y轴方向上的偏移</td>
        </tr>
        <tr>
            <td>text-anchor</td>
            <td>描述该文本与所给点的对齐方式 (开头、中间、末尾对齐)</td>
        </tr>
        <tr>
            <td>rotate</td>
            <td>指定动画元素沿<animateMotion>元素中指定的路径行进时如何旋转。</td>
        </tr>
        <tr>
            <td>textLength</td>
            <td>文本要绘制到的空间的宽度</td>
        </tr>
        <tr>
            <td>lengthAdjust</td>
            <td>控制如何将文本拉伸为textLength属性定义的长度。</td>
        </tr>
        <tr>
            <td rowspan="1" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;G&gt;</td>
            <td>用来组合对象的容器</td>
            <td></td>
            <td>添加到g元素的属性会被其所有的子元素继承。此外，g元素也可以用来定义复杂的对象，之后可以通过<use>元素来引用它们。</td>
        </tr>
        <tr>
            <td rowspan="2" style="text-align:center;font-weight:bold;color:#CF57FF">&lt;PATH&gt;</td>
            <td rowspan="2" >用来定义形状的通用元素</td>
            <td>d</td>
            <td>定义了一个路径</td>
        </tr>
        <tr>
            <td>pathLength</td>
            <td>允许作者以用户单位指定路径的总长度。然后，通过使用pathLength/（路径长度的计算值）比率缩放所有距离计算，使用该值与作者的浏览器距离计算进行校准</td>
        </tr>
    </tbody>
</table>

### <p style='color:#07823A'>3. &lt;path>的路径指令</p>

<table>
<thead style="color:#fff;font-weight:bold;">
    <tr style="background:#03a9f4;">
        <th>路径指令</th>
        <th>描述</th>
        <th>语法</th>
        <th>属性含义</th>
    </tr>
</thead>
<tbody>
    <tr >
        <td rowspan="2">M（Moveto）</td>
        <td rowspan="2">可以被想象成拎起绘图笔，落脚到另一处</td>
        <td>M x,y </td>
        <td>x和y是绝对坐标。位于绝对位置x=50, y= 100：&lt;path d="M50,100..." /&gt;</td>
    </tr>
    <tr >
        <td>m dx,dy </td>
        <td>dx和dy是相对于当前点的距离。往右移50，往下移100：&lt;path d="m50,100..." /&gt;</td>
    </tr>
    <tr >
        <td rowspan="2">L（Lineto）</td>
        <td rowspan="2">绘制一条直线段。这个直线段从当前位置移到指定位置</td>
        <td>L x, y </td>
        <td>x和y是绝对坐标，分别代表水平坐标和垂直坐标</td>
    </tr>
    <tr >
        <td>l dx, dy </td>
        <td>dx和dy是相对于当前点的距离，分别是向右和向下的距离</td>
    </tr>
    <tr >
        <td >H</td>
        <td >水平移动</td>
        <td>H:绝对位置,h:相对距离</td>
        <td>从当前点画水平线到指定y轴座标</td>
    </tr>
    <tr >
        <td >V</td>
        <td >垂直移动</td>
        <td>H:绝对位置,h:相对距离</td>
        <td>从当前点画垂直线到指定y轴座标</td>
    </tr>
    <tr >
        <td rowspan="2">C（Curveto）</td>
        <td rowspan="2">贝塞尔曲线:立方曲线</td>
        <td>C c1x,c1y c2x,c2y x,y</td>
        <td>c1x、c1y和c2x、c2y是分别是初始点和结束点的控制点的绝对坐标</td>
    </tr>
    <tr >
        <td >c dc1x,dc1y dc2x,dc2y dx,dy</td>
        <td>dc1x、dc1y和dc2x、dc2y都是相对于初始点，而不是相对于结束点的。dx和dy分别是向右和向下的距离</td>
    </tr>
    <tr >
        <td rowspan="2">Q（Curveto）</td>
        <td rowspan="2">贝塞尔曲线:二次方曲线</td>
        <td>Q cx, cy  x, y</td>
        <td>cx和cy都是控制点的绝对坐标</td>
    </tr>
    <tr >
        <td >q dcx, dcy dx, dy</td>
        <td>dcx和dcy分别是控制点在x和y方向上的距离</td>
    </tr>
    <tr >
        <td >S</td>
        <td >贝塞尔曲线</td>
        <td>S cx,cy x,y</br>s dcx,dcy dx,dy</td>
        <td>从目前点的座标画条反射的贝塞尔曲线到指定点的x,y座标，其中(d)cx为反射控制点，即指定第二个控制点</td>
    </tr>
    <tr >
        <td >T</td>
        <td >二次贝塞尔曲线</td>
        <td>T x,y</br>t dx,dy</td>
        <td>从目前点的座标画条反射二次贝塞尔曲线到指定点的x,y座标，以前一个座标为反射控制点。如果没有前一个控制点的话它实际上就是前一个点</td>
    </tr>
    <tr >
        <td >A（Arcto）</td>
        <td >一个椭圆弧曲线路径</td>
        <td>A rx ry x-axis-rotation large-arc-flag sweep-flag x y</td>
        <td>* rx和ry分别是x和y方向的半径</br>
            * x-axis-rotation 是弧线与x 轴的旋转角度</br>
            * LargeArcFlag的值要到是0要么是1，用来确定是要画小弧（0）还是画大弧（1）</br>
            * SweepFlag也要么是0要么是1，用来确定弧是顺时针方向（1）还是逆时针方向（0)</br>
            * x和y是目的地的坐标
        </td>
    </tr>
    <tr >
        <td>Z/z(ClosePath)</td>
        <td>关闭路径</td>
        <td></td>
        <td>将当前点和第一个点座标连起来</td>
    </tr>

</tbody>
</table>

### <p style='color:#07823A'>4. 代码示例</p>
````
<svg xmlns="http://www.w3.org/2000/svg" width="1000px" height="1000px">
    <circle cx="50" cy="50" r="45" style="fill:green"></circle>
    <rect x="150" y="150" width="100" height="100" rx="15" ry="15" style="fill:yellow"></rect>
    <line x1="100" x2="350" y1="350" y2="390" stroke="red"></line>
    <polygon points="100,10 250,150 200,110" style="fill:orange;stroke:green;stroke-width:1"></polygon>
    <text x="160" y="200" dx="0" dy="10" transform="rotate(30 20,40)">
        SVG Text Rotation example
    </text>
    <text text-anchor="start" x="60" y="180">ABC</text>
    <text text-anchor="middle" x="60" y="200">DEF</text>
    <text text-anchor="end" x="60" y="220">GHI</text>
    <text x="300" y="100" textLength="6em">Small text length</text>
    <text x="300" y="140" textLength="30%">Big text length</text>
    <text x="300" y="180" textLength="300" lengthAdjust="spacing">
      Stretched using spacing only.
    </text>
    <text x="300" y="220" textLength="300" lengthAdjust="spacingAndGlyphs">
      Stretched using spacing and glyphs.
    </text>
    <text x="300" y="260" textLength="100" lengthAdjust="spacing">
      Shrunk using spacing only.
    </text>
    <text x="300" y="300" textLength="100" lengthAdjust="spacingAndGlyphs">
      Shrunk using spacing and glyphs.
    </text>
    <g stroke="pink" fill="white" stroke-width="5">
        <circle cx="425" cy="55" r="15"></circle>
        <circle cx="440" cy="55" r="15"></circle>
        <circle cx="455" cy="55" r="15"></circle>
        <circle cx="470" cy="55" r="15"></circle>
    </g>
    <path d="M 100 400 L 300 400 L 200 600 z" fill="#CF57FF" stroke="#FBA1F6" stroke-width="2"></path>
    <path d="M 400 150 C 400 0 650 0 650 150 H 800" style="fill:none;stroke:#03a9f4;stroke-width:2;"></path>
    <path d="M 400 350 Q 650 250 650 350 T 800 350" style="fill:none;stroke:red;stroke-width:5;"></path>
</svg>
````
![PNG](/asset/img/svgdemo.png)
[参考文档:MDN web docs-SVG](https://developer.mozilla.org/zh-CN/docs/Web/SVG)
