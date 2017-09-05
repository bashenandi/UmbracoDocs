#什么是网格布局?
要理解网格布局编辑器是如何工作的，必须先理解网格布局的结构。

网格布局包含两个需要配置的主要区域，*grid layout area* 和 *grid rows*。

###网格布局 
*layout area*定义了整体的网页布局。*Layout areas*由*layout sections*进行分割，例如：sidebar 区块和内容区块。*layout sections*的尺寸由列来进行定义。对于100%宽度的内容区域来说，能够使用最大的列是（Bootstrap3中是12列）。每个*layout section* 都包含一个或多个*rows*。

![Grid rows](images/Grid-layout-rows.jpg)

###网格中的行
网格中的*rows*是实际的内容所在。每一行都由*cells*分隔，而属性编辑器就包含在其中。单元格的尺寸由列所定义。不同于*layouts sections*，可以添加更多的*cells*甚至超过列的数目 - 它们将堆叠在网格系统中。

![Grid structure](images/Grid-layout-NO-SIDEBAR-rows.jpg)