1. http是一个简单的请求-响应协议，是一个无状态的超文本传输协议。
2. Cookie，类型为“小型文本文件”，不超过4KB。是某些网站为了辨别用户身份，进行Session跟踪而储存在用户本地终端上的数据（通常经过加密），由用户客户端计算机暂时或永久保存的信息
3. 组成
* Name/Value：设置Cookie的名称及相对应的值，对于认证Cookie，Value值包括Web服务器所提供的访问令牌。
* Domain属性：指定了可以访问该 Cookie 的 Web 站点或域。Cookie 机制并未遵循严格的同源策略，允许一个子域可以设置或获取其父域的 Cookie。当需要实现单点登录方案时，Cookie 的上述特性非常有用，然而也增加了 Cookie受攻击的危险，比如攻击者可以借此发动会话定置攻击。因而，浏览器禁止在 Domain 属性中设置 .org、.com 等通用顶级域名、以及在国家及地区顶级域下注册的二级域名，以减小攻击发生的范围。
* Expires属性：设置Cookie的生存期。有两种存储类型的Cookie：会话性与持久性。Expires属性缺省时，为会话性Cookie，仅保存在客户端内存中，并在用户关闭浏览器时失效；持久性Cookie会保存在用户的硬盘中，直至生存期到或用户直接在网页中单击“注销”等按钮结束会话时才会失效。（不建议用Max-age属性了）（如果设置为过去的时间，会导致cookie删除失效）
* Path属性：定义了Web站点上可以访问该Cookie的目录。
* Secure属性：指定是否使用HTTPS安全协议发送Cookie。使用HTTPS安全协议，可以保护Cookie在浏览器和Web服务器间的传输过程中不被窃取和篡改。该方法也可用于Web站点的身份鉴别，即在HTTPS的连接建立阶段，浏览器会检查Web网站的SSL证书的有效性。但是基于兼容性的原因（比如有些网站使用自签署的证书）在检测到SSL证书无效时，浏览器并不会立即终止用户的连接请求，而是显示安全风险信息，用户仍可以选择继续访问该站点。由于许多用户缺乏安全意识，因而仍可能连接到Pharming攻击所伪造的网站。
* HTTPOnly 属性 ：用于防止客户端脚本通过document.cookie属性访问Cookie，有助于保护Cookie不被跨站脚本攻击窃取或篡改。但是，HTTPOnly的应用仍存在局限性，一些浏览器可以阻止客户端脚本对Cookie的读操作，但允许写操作；此外大多数浏览器仍允许通过XMLHTTP对象读取HTTP响应中的Set-Cookie头。


```
// 获取
document.cookie // name1=value1;name2=value2;...
// 新增
document.cookie = encodeURIComponet('key1') + '=' + encodeURIComponet('value1') + ';domain=baidu.com' + ';expires=' + new Date('2020-01-01');
// 删除 设置为过去的时间 new Date(0) 为 1970-01-01
document.cookie = encodeURIComponet('key1') + '=' + encodeURIComponet('value1') + ';domain=baidu.com' + ';expires=' + new Date(0);

// 便捷的类库 js-cookie
Cookies.set('name', 'value', opts);
Cookies.get('name');
Cookies.remove('name')

```
