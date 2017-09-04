#文件和文件夹权限

_为了稳定和顺利的运行Umbraco 安装程序，有些权限需要正确的设置。这些权限应该在安装 Umbraco 之前或者之后设置。需要设置权限的用户，是网站使用应用程序池的拥有者（通常是 **Network Service**或者**IIS\\\_IUSERS**组）。如果有任何疑问，请咨询你的服务器管理员或者主机供应公司。一般来说如果你使用 WebMatrix，权限并不需要严格的设置。_

<table border="0" style="margin-top:20px;">
<thead>
<tr>
<th>文件 / 目录</th>
<th>权限</th>
<th>注释</th>
</tr>
</thead>

<tbody>
<tr>
<th>/Web.config</th>
<th>Modify / Full control</th>
<td>
<p>仅需要在安装期间设置数据库和版本信息。因此，为了增强安全性，可以在完成后，将其设为只读。</p>
</td>
</tr>
<tr>
<th>/App_Code</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于动态加载以及生成 dll。</p>
</td>
</tr>
<tr>
<th>/App_Data</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于缓存和存储。</p>
</td>
</tr>
<tr>
<th>/App_Plugins</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于打包。</p>
</td>
</tr>
<tr>
<th>/Bin</th>
<th>Modify / Full control</th>
<td>
<p>安装软件包则需要可写，如果没有软件包需要安装，可以设置为只读权限。</p>
</td>
</tr>
<tr>
<th>/Config</th>
<th>Modify / Full control</th>
<td>
<p>仅需要在安装期间设置数据库和版本信息。因此，为了增强安全性，可以在完成后，将其设为只读。</p>
</td>
</tr>
<tr>
<th>/Css</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于 css 文件。</p>
</td>
</tr>
<tr>
<th>/Masterpages</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于模板文件。</p>
</td>
</tr>
<tr>
<th>/Media</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于通过 Umbraco cms 界面来上传文件。</p>
</td>
</tr>
<tr>
<th>/Scripts</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于 js 文件。</p>
</td>
</tr>
<tr>
<th>/Umbraco</th>
<th>Modify / Full control</th>
<td>
<p>在更新和安装软件包时需要修改权限，但是在结束后可以设置为只读。</p>
</td>
</tr>
<tr>
<th>/Umbraco_client</th>
<th>Modify / Full control</th>
<td>
<p>在更新和安装软件包时需要修改权限，但是在结束后可以设置为只读。</p>
</td>
</tr>
<tr>
<th>/UserControls</th>
<th>Modify / Full control</th>
<td>
<p>安装软件包时设置修改权限</p>
</td>
</tr>
<tr>
<th>/Views</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于模板文件，视图、局部视图和宏文件均存储在这里</p>
</td>
</tr>
<tr>
<th>/Xslt</th>
<th>Modify / Full control</th>
<td>
<p>需要一直设置为修改权限，用于宏文件。</p>
</td>
</tr>
</tbody>
</table>
