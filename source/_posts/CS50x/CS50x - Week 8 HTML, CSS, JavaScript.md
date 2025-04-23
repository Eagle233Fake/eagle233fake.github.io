---
title: CS50x - Week 8 HTML, CSS, JavaScript
date: 2024-11-28 14:09:30
tags:
categories: 
- CS50x
---

---

**[Stanford CS144: Computer Network - CS自学指南](https://csdiy.wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/CS144/)**

---

## Internet

### IP (Internet Protocol) Address

- **32-bit** address (**IPv4**) as **four** clusters of **8-bits** using **decimal** notation.
      w.x.y.z
- Each of w, x, y, and z can be a nonnegative value in the range **[0, 255]**.
      140.247.223.81

- Roughly **4 billion** addresses to give out.
- The population of the world is somewhere in excess of **7 billion**, and most folks in the western world have more than 1 device capable of Internet connectivity.
  - Some workarounds have allowed us to deal with this problem (for now).

#### IPv6

- **128-bit addresses**
      s:t:u:v:w:x:y:z
- Each of s, t, u, v, w, x, y, and z is represented by **1 to 4 hexadecimal digits** in the range **[0,ffff]**.
      1234:5678:90ab:cdef:fedc:ba09:8765:4321
      2001:4860:4860:0:0:0:0:8844
      2001:4860:4860::8844

### DHCP (Dynamic Host Configuration Protocol)

- Somewhere between your computer and the Internet at large exists a DHCP server, whose role is to assign IP addresses to devices.

### DNS (The Domain Name System)

- Help us translate IP addresses to more memorable names that are more humancomprehensible.

| Host Name                | IPv4 Address     |
|--------------------------|------------------|
| info.host1.net          | 0.0.0.0          |
| info.host2.net          | 0.0.0.1          |
| ...                      | ...              |
| io-in-f138.1e100.net (google.com)   | 74.125.202.138    |
| ...                      | ...              |
| info.hostn-1.net        | 255.255.255.254  |
| info.hostn.net          | 255.255.255.255  |

| Host Name                | IPv6 Address                     |
|--------------------------|----------------------------------|
| info.host1.net          | 0:0:0:0:0:0:0:0                  |
| info.host2.net          | 0:0:0:0:0:0:0:1                  |
| ...                      | ...                              |
| lk-in-x93.1e100.net (google.ie)    | 2a00:1450:4010:c09:0:0:0:93      |
| ...                      | ...                              |
| info.hostn-1.net        | ffff:ffff:ffff:ffff:ffff:ffff:ffff:fffe |
| info.hostn.net          | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff |

- DNS record sets are thus fairly decentralized.

### Access Points

- The IP address is assigned to a ***router***, whose job it is to act as a traffic cop that allows data requests from all of the devices on your local network (your home or business, e.g.) to be processed through a single IP address.
  - Router: **Public IP Address**
  - People: **Private IP Address**
- Modern home networks consist of access points that combine a router, a modem, a switch, and other technologies together into a single device. Modern business networks or large-scale wide-area networks **(WANs)** still frequently have these as separate devices to allow the size of their network to scale more easily.

### Protocols

![internet_00.png](https://s2.loli.net/2024/11/28/IzbwjpQaKfJBC2d.png)
![internet_01.png](https://s2.loli.net/2024/11/28/J8yfBth3v1QIMHl.png)

---

## IP (Internet Protocol)

- We can't afford to wire each network to each other network.
![ip_00.png](https://s2.loli.net/2024/11/28/mrNo5qJ37WTjpXc.png)

### Routers

- This information might be stored in a **routing table**, inside of the router.
- **Recursion**
- Reduce thecosts of network infrastructure.
![ip_01.png](https://s2.loli.net/2024/11/28/hcWwkxqd8aDrFHi.png)

### Packets

- Data isn't being sent as one huge block.
- Any slowdown that was caused by sending such a large amount of data would have a ripple effect that would throttle the network for all the other users.
- And breaking the data into small pieces would reduce the side effect of losing data.
- As such, another crucial part of IP is splitting data into packets.

### Connectionless Protocol

- There is not necessarily a defined path from the sender to the receiver, and vice versa (反之亦然).
![ip_02.png](https://s2.loli.net/2024/11/28/rHAVU7xc9nZ4RXf.png)
- If "clogging", being **"re-routed"** to follow the most optimal path, based on the current state of the network.
- So **routing table** contain not only the receivers' addresses but also the cost of using a particular route.
- What if we do drop a packet?
  - Delivery cannot be guaranteed, since the path from sender to receiver is not guaranteed to be consistent.
  - Talk about **Transmission Control Protocol (TCP)** in the next short.

---

## TCP (Transmission Control Protocol)

- Directing the transmitted packet to **the correct program** on the receiving machine.
- Identify both where the receiver is and what the packet is for, so TCP and IP are almost an inseparable pair: **TCP/IP**.
  - Each program / utility / service on a machine is assigned a **port** number. Coupled with an IP address, we can now uniquely identify a specific program on a specific machine.
  - **Guaranteeing delivery** of packets, which IP alone does not do.
- TCP does this by including information about **how many packets** the receiver should expect to get, and **in what order**, and transmitting **that information alongside the data**.

### Standardized Ports

| Protocol        | Port Number |
|------------------|-------------|
| FTP (File Transfer) | 21          |
| SMTP (E-mail)       | 25          |
| DNS (Domain Name System) | 53          |
| HTTP (Web Browsing) | 80          |
| HTTPS (Secure Web Browsing) | 443         |

### Steps of the TCP/IP Process

1. **Data Segmentation**: When a program sends data, TCP **breaks** it into smaller chunks and adds a **TCP layer** to each packet.
2. **Routing**: IP routes the individual packets from sender to receiver, including routing information in the **IP layer**.
3. **Reassembly**: Upon receiving the packets, TCP checks the header to identify the destination program and reorders the packets as needed before delivering them.

- **Packet Loss Recovery**: If a router drops a packet during transmission, TCP uses information in the packet headers to request the sender to resend the missing packet for complete assembly.
- **Reassembly and Delivery**: Once all packets arrive, TCP organizes them in the correct order and reassembles them into the intended data unit before delivering it to the appropriate service.
![tcp_00.png](https://s2.loli.net/2024/11/28/IOoZSW6nAECYsVm.png)

---

## HTTP (Hypertext Transfer Protocol)

- HTTP is one example of an **application layer protocol**, which specifically dictates the **format** by which clients **request** web pages from a server, and the **format** via which servers **return** information to clients.

### Other Application Layer Protocols

- File Transfer Protocol (FTP)
- Simple Mail Transfer Protocol (SMTP)
- Data Distribution Service (DDS)
- Remote Desktop Protocol (RDP)
- Extensible Message and Presence Protocol (XMPP)

### Form

![http_00.png](https://s2.loli.net/2024/11/30/jugDskVC56z1tL7.png)
![http_01.png](https://s2.loli.net/2024/11/30/oC3zde7qhubIPf2.png)

`method request-target http-version`
`GET request-target HTTP/1.1`
`POST request-target HTTP/1.1`

### Status Codes

`http-version status`

| Class Code | Text                      | Comments                                                        |
|------------|---------------------------|-----------------------------------------------------------------|
| Success    | 200 OK                    | All is well, valid request and response.                        |
| Redirection| 301 Moved Permanently     | Page is now at a new location, automatic redirects built in to most browsers. |
|            | 302 Found                 | Page is now at a new location temporarily.                     |
| Client Error| 401 Unauthorized          | Page typically requires login credentials.                     |
|            | 403 Forbidden              | Server will not allow this request.                            |
|            | 404 Not Found              | Server cannot find what was asked for.                        |
| Server Error| 500 Internal Server Error | Generic server failure in responding to the otherwise-valid request. |

### Developer Tools

`F12 - Network`
![http_02.png](https://s2.loli.net/2024/11/30/To9PYqrHlR73spu.png)

---

## HTML (Hypertext Markup Language)

```bash
$ http-server
```

- HTML (Hypertext Markup Language) is a fundamental component of every website.
- HTML is a language, but it is **not a programming language**. It lacks concepts of variables, logic, functions, and the like.
- Rather, it is a **markup language**, using angle-bracket enclosed tags to semantically define the structure of a web page, causing the plain text inside of sets of tags to be interpreted by web browsers in different ways.

```html
<html lang="en">
  <head>
    <title> <!-- The title on the top of the browser -->
      Hello, world
    </title>
  </head>
  <body> <!-- The main content of the webpage -->
    World, hello
  </body>
</html>
```

```html
<html><head><title>Hello,world</title></head><body>World,hello</body></html> <!-- Indentation does't really matter -->
```

### Common HTML Tags

| 标签                     | 描述                                                                                     |
|------------------------|----------------------------------------------------------------------------------------|
| `<b>, </b>`           | 文本将以**粗体**显示。                                                                       |
| `<i>, </i>`           | 文本将以***斜体***显示。                                                                       |
| `<u>, </u>`           | 文本将被**下划线**显示。                                                                     |
| `<p>, </p>`           | 文本将作为**段落**显示，段落**上方和下方有空白**。                                               |
| `<hX>, </hX>`         | X = 1, 2, 3, 4, 5, 或 6，文本将作为**X级标题**显示。                                         |
| `<ul>, </ul>`         | 标记**无序（项目符号）列表**的开始和结束。                                                  |
| `<ol>, </ol>`         | 标记**有序（编号）列表**的开始和结束。                                                      |
| `<li>, </li>`         | 标记***有序或无序列表中的列表项**。                                                          |
| `<table>, </table>`   | 标记**表格定义**的开始和结束。                                                              |
| `<tr>, </tr>`         | 标记**表格中一行**的开始和结束。**(Table Row)**                                                            |
| `<td>, </td>`         | 标记**表格中一行内一列**的开始和结束。**(Table Data)**                                                      |
| `<form>, </form>`     | 标记**HTML表单**的开始和结束。                                                              |
| `<div>, </div>`       | 标记**HTML页面中任意部分**的开始和结束。                                                    |
| `<input name=X type=Y />` | 定义**HTML表单中的字段**。X是该字段的**唯一标识符**，Y是接受的**数据类型**。                     |
| `<a href=X>, </a>`   | 创建指向网页X的[超链接]()，标签之间的文本将作为链接文本显示。                             |
| `<img src=X ... />`  | 显示位于X的**图像**的自闭合标签，可能有其他属性（如指定宽度和高度）。                     |
| `<!DOCTYPE html>`     | 特定于HTML5，告知**浏览器使用的标准**。                                                     |
| `<!--, -->`           | 标记HTML**注释**的开始和结束。                                                              |

- Beyond the tags as explained here, each can also have multiple attributes that slightly modify the tag.
  - For example, you can usually add an `id=X` attribute, to uniquely identify a tag within an overall page.

### Attentions

- Everytag you open should be closed (unless it is a self-closing tag), and tags should be closed in reverse order of when they were
opened.
- Not necessarily fail with **syntax errors** if not well-formed, so it's up to you to be vigilant.
- Be sure to use **online HTML validators** to help.

### Elements

| HTML 元素  | 描述                             |
|------------|----------------------------------|
| `header`   | 表示介绍性内容或导航链接。       |
| `main`     | 表示文档的主要内容。             |
| `footer`   | 表示其最近的区块内容的页脚。     |

- Just for readability.

```html
<html>
  <head>
    <title>
      Hello, world
    </title>
  </head>
  <body>
    <header>
      Titles Goes there
    </header>
    <main>
      World, hello
    </main>
    <footer>
      Made by Eagle233
    </footer>
  </body>
</html>
```

---

## CSS (Cascading Style Sheets)

- The tool we use to customize our website's look and feel.
- Like HTML, CSS is not a programming language; it lacks logic. Rather, it is a styling language and its syntax describes how certain attributes of HTML elements should be modified.

```css
body
{
  background-color: blue;
}
```

- A style sheet is constructed by identifying a **selector** (in the last example, `body`) and then an open curly brace to indicate the beginning of the style sheet for that selector.
- In between the curly brace, you place a list of **key-value pairs** of style properties and values for those properties, each declaration ending with a semicolon.
- Then a closing curly brace terminates the style sheet.

### Common CSS properties

```css
border: style color width
```

- Applies a **border** of the specified color, width, and style (e.g., dotted, dashed, solid, ridge…).

```css
background-color: [keyword | #<6-digit hex>]
```

- Sets the **background color**. Some colors are pre-defined in CSS.

```css
color: [keyword | #<6-digit hex>]
```

- Sets the **foreground color** (usually text).

```css
font-size: [absolute size | relative size]
```

- Can use **keywords (xx-small, medium…)**, **fixed points (10pt, 12pt…)**, **percentage (80%, 120%)**, or base off **the most recent font size (smaller, larger)**.

```css
font-family: [font name | generic name]
```

- Certain **"web safe"** fonts are pre-defined in CSS.

```css
text-align: [left | right | center | justify]
```

- For displaying text.

### Selectors

#### Tag Selector

```css
h2
{
  font-family: times;
  color: #fefefe;
}
```

#### ID Selector

- Apply only to an HTML tag with a unique identifier.

```css
#unique
{
  border: 4px dotted blue;
  text-align: right;
}
```

#### Class selector

- A class selector will apply only to those HTML tags that have been given identical "class" attributes.

```css
.students
{
  background-color: yellow;
  opacity: 0.7;
}
```

### Two Options For Writing Stylesheets

- Style sheets can be written directly into your HTML.
  - Place them within `<style>` tags within your page's head.

```html
<style>
  /* Your CSS rules here */
</style>
```

- Style sheets can also be written as separate CSS files and then linked into your document.
- Use `<link>` tags within your page's head to accomplish this.

```html
<link rel="stylesheet" href="styles.css">
```

### Comments

```css
/* Your CSS rules here */
```

---

## JavaScript

- JavaScript, HTML, and CSS make up the three languages defining most of the user experience on the web.
- JavaScript applications run **client-side**, on your own machine.

### Including JavaScript in HTML

- Just like CSS with `<style>` tags, you can directly write your JavaScript between `<script>` tags.
- Just like CSS with `<link>` tags, you can write your JavaScript in separate files and link them in by using the src attribute of the `<script>` tag.

### Variables

- No type specifier.
- When a local variable is first declared, preface with the `var` keyword.
- 使用 `let` 更加安全，因为它具有块作用域，避免了变量提升和重复声明的问题。
- 在现代 JavaScript 开发中，推荐使用 `let` 和 `const`（用于声明常量）来替代 `var`，以提高代码的可读性和可维护性。

```js
var x = 44;
let d = 1;
```

### Conditionals

- All of the old favorites from C are still available for you to use.

```js
if
else if
else
switch
?:
```

### Loops

- All of the old favorites from C are still available for you to use.

```js
while
do-while
for
```

### Functions

- All functions are introduced with the `function` keyword.
- Can be **anonymous**.

### Arrays

```js
var nums = [1, 2, 3, 4, 5];
var mixed = [1,
            true,
            3.333,
            'five'];
```

### Objects

- JavaScript has the ability to behave as an **object-oriented programming language**.
- Have properties but also **methods**, or
**functions** that are inherent to the object, and mean nothing outside of it. Thus, like properties can methods not ever stand on their own.

```js
object.function();
var herbie = {year : 1963, model: 'Beetle'};
```

### Loops (redux)

- **Iterate** across all of the key-value pairs of an object:

```js
for (var key in object)
{
  // use object[key] in here
  // key
}
```

```js
for (var key of object)
{
  // use key in here
  // value
}
```

```js
var wkArray = ['Monday',
              'Tuesday',
              'Wednesday',
              'Thursday',
              'Friday',
              'Saturday',
              'Sunday'];
for (var day in wkArray)
{
  console.log(day);
}

for (var day of wkArray)
{
  console.log(day);
}
```

```js
console.log(wkArray[day] + ' is day number ' + (day + 1) + ' of the week!'); // day and 1 are all numbers
```

```js
console.log(wkArray[day] + ' is day number ' + (parseInt(day) + 1) + ' of the week!');
```

### Functions (redux)

- **Everything** in JavaScript is a special case of an object.

```js
array.size();
array.pop();
array.push(x);
array.shift();
```

#### `map()`

- Apply a function to all elements of an array.

- A great situation to use an **anonymous** function.

```js
var nums = [1, 2, 3, 4, 5];
nums = nums.map(function(num)
{
  return num * 2;
});
```

### Events

- A response to user interaction with the web page.
  - A user clicks a button, a page has finished loading, a user has hovered over a portion of the page, the user typed in an input field.
- Support for **event handlers**, which are callback functions that respond to HTML events.
  - Many HTML elements have support for events as an attribute.

```html
<html>
  <head>
    <title>Event Handlers</title>
  </head>
  <body>
    <button onclick="alertName(event)">Button 1</button>
    <button onclick="alertName(event)">Button 2</button>
  </body>
</html>
```

```js
function alertName(event)
{
  var trigger = event.srcElement;
  alert('You clicked on ' + trigger.innerHTML);
}
```

### Implementation

#### Detection

```js
// 只选择第一个元素
document.querySelector('#add1').onclick = function() {

}
document.querySelector('#score1').innerHTML = function() {

}
```

```js
// 选择多个元素
document.querySelectorAll('#add1, #add2').forEach(function(element) {
  element.onclick = function() {
  }
}
```

#### Alert

```js
if (team == 4)
{
  alert("Team won!");
}
```

#### Modify the Paragraph

```js
if (team == 5)
{
  document.querySelector("#result").innerHTML = "Team won!";
}
```

### 外部引用 JS

```html
<script src="script.js"></script>
```

---

## DOM (Document Object Model)

- By organizing an entire page into a JavaScript object, we can manipulate the page's elements programmatically.

```html
<html>
  <head>
    <title>Hello, world</title>
  </head>
  <body>
    <h2>Here's my page</h2>
    <p>World, hello</p>
    <a href="test.html">Link</a>
  </body>
</html>
```

![dom_00.png](https://s2.loli.net/2024/11/30/NCgJ2qhkfw8QUOu.png)
- The contents of our web pages can change without us needing to refresh the page.

```js
document.addEventListener('DOMContentLoaded', function() {
    document.querySelectorAll('#1, #2, #3, #4').forEach(function(element) {
        element.onclick = function() {
            document.querySelector('#iscorrect').innerHTML = 'Incorrect';
            element.style.color = 'red';
        }
    });

    document.querySelector('#5').onclick = function() {
        document.querySelector('#iscorrect').innerHTML = 'Correct!';
        document.querySelector('#5').style.color = 'green';
    }
});
```

- The `document.addEventListener('DOMContentLoaded', function() { ... });` line ensures that the JavaScript code inside the function runs only after the HTML document has been fully loaded. This prevents errors that occur when trying to access elements that haven't been created yet.

### DOM Properties

| DOM Property   | Description                                                  |
|----------------|--------------------------------------------------------------|
| `innerHTML`    | Holds the HTML inside a set of HTML tags.                   |
| `nodeName`     | The name of an HTML element or element's attribute.          |
| `id`           | The "id" attribute of an HTML element.                       |
| `parentNode`   | A reference to the node one level up in the DOM.            |
| `childNodes`   | An array of references to the nodes one level down in the DOM. |
| `attributes`   | An array of attributes of an HTML element.                   |
| `style`        | An object encapsulating the CSS/HTML styling of an element.  |

### DOM Methods

| DOM Method                     | Description                                                  |
|--------------------------------|--------------------------------------------------------------|
| `getElementById(id)` (`Id` but not `ID`)          | Gets the element with a given ID below this point in the DOM. |
| `getElementsByTagName(tag)`   | Gets all elements with the given tag below this point in the DOM. |
| `appendChild(node)`            | Adds the given node to the DOM below this point.            |
| `removeChild(node)`            | Removes the specified child node from the DOM.               |

### [jQuery](https://api.jquery.com/)

- A popular open-source library, released in 2006, that is designed to simplify client-side scripting (such as DOM manipulations).

```js
document.getElementById('colorDiv').style.backgroundColor = 'green'
```

```js
$('#colorDiv').css('background-color', 'green');
```

---
