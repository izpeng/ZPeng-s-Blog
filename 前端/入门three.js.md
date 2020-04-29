突然感觉three.js挺有意思的，简单入门了一下three.js，但是并没有打算深入学习。
## 介绍
&emsp;&emsp;three.js是JavaScript编写的WebGL第三方库。提供了非常多的3D显示功能。
&emsp;&emsp;Three.js 是一款运行在浏览器中的 3D 引擎，你可以用它创建各种三维场景，包括了摄影机、光影、材质等各种对象。你可以在它的主页上看到许多精彩的演示。不过，这款引擎还处在比较不成熟的开发阶段，其不够丰富的 API 以及匮乏的文档增加了初学者的学习难度（尤其是文档的匮乏）three.js的代码托管在github上面。
![image.png](https://www.zhangpeng.fun/upload/2020/04/image-1481693e51244af7af24d3ec2ace6dfb.png)
官网：[https://threejs.org/](https://threejs.org/)

Github：[https://github.com/mrdoob/three.js/](https://github.com/mrdoob/three.js/)

目录结构：
Build目录：包含两个文件，three.js 和three.min.js 。这是three.js最终被引用的文件。一个已经压缩，一个没有压缩的js文件。

Docs目录：这里是three.js的帮助文档，里面是各个函数的api，可惜并没有详细的解释。试图用这些文档来学会three.js是不可能的。

Editor目录：一个类似3D-max的简单编辑程序，它能创建一些三维物体。

Examples目录：一些很有趣的例子demo，可惜没有文档介绍。对图像学理解不深入的同学，学习成本非常高。

Src目录：源代码目录，里面是所有源代码。

Test目录：一些测试代码，基本没用。

Utils目录：存放一些脚本，python文件的工具目录。例如将3D-Max格式的模型转换为three.js特有的json模型。

.gitignore文件：git工具的过滤规则文件，没有用。

CONTRIBUTING.md文件：一个怎么报bug，怎么获得帮助的说明文档。

LICENSE文件：版权信息。

README.md文件：介绍three.js的一个文件，里面还包含了各个版本的更新内容列表。
## 入门程序
1. 下载three.js
2. 导入three.js
```language
<script src="js/three.js"></script>
```

3. 三大组件
在Three.js中，要渲染物体到网页中，我们需要3个组建：场景（scene）、相机（camera）和渲染器（renderer）。有了这三样东西，才能将物体渲染到网页中去。
创建这三要素的代码如下：
```language
var scene = new THREE.Scene();	// 场景
var camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);// 透视相机
var renderer = new THREE.WebGLRenderer();	// 渲染器
renderer.setSize(window.innerWidth, window.innerHeight);	// 设置渲染器的大小为窗口的内宽度，也就是内容区的宽度
document.body.appendChild(renderer.domElement);
```
     

在Threejs中场景就只有一种，用THREE.Scene来表示，要构件一个场景也很简单，只要new一个对象就可以了，代码如下：
```language
var scene = new THREE.Scene(); 
```
4. 相机
另一个组建是相机，相机决定了场景中那个角度的景色会显示出来。相机就像人的眼睛一样，人站在不同位置，抬头或者低头都能够看到不同的景色。

场景只有一种，但是相机却又很多种。和现实中一样，不同的相机确定了呈相的各个方面。比如有的相机适合人像，有的相机适合风景，专业的摄影师根据实际用途不一样，选择不同的相机。对程序员来说，只要设置不同的相机参数，就能够让相机产生不一样的效果。

```language
 var camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
```
5.渲染器
最后一步就是设置渲染器，渲染器决定了渲染的结果应该画在页面的什么元素上面，并且以怎样的方式来绘制。这里我们定义了一个WebRenderer渲染器，代码如下所示：

```language
var renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);
```

6.渲染
渲染应该使用渲染器，结合相机和场景来得到结果画面。实现这个功能的函数是

renderer.render(scene, camera);

渲染函数的原型如下：

render( scene, camera, renderTarget, forceClear )

各个参数的意义是：

scene：前面定义的场景

camera：前面定义的相机

renderTarget：渲染的目标，默认是渲染到前面定义的render变量中

forceClear：每次绘制之前都将画布的内容给清除，即使自动清除标志autoClear为false，也会清除。

7.完整代码
```language
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style>canvas { width: 100%; height: 100% }</style>
    <script src="js/three.js"></script>
</head>
<body>
    <script>
        var scene = new THREE.Scene();
        
        var camera = new THREE.PerspectiveCamera(75, window.innerWidth/window.innerHeight, 0.1, 1000);
        
        var renderer = new THREE.WebGLRenderer();
        
        renderer.setSize(window.innerWidth, window.innerHeight);
        
        document.body.appendChild(renderer.domElement);
        var geometry = new THREE.CubeGeometry(1,1,1);
        var material = new THREE.MeshBasicMaterial({color: 0x00ff00});
        var cube = new THREE.Mesh(geometry, material); scene.add(cube);
        camera.position.z = 5;
        function render() {
            requestAnimationFrame(render);
            cube.rotation.x += 0.1;
            cube.rotation.y += 0.1;
            renderer.render(scene, camera);
        }
        render();
    </script>
</body>
</html>
```
## 示例
官网全部示例：[https://threejs.org/examples/#webgl_animation_cloth](https://threejs.org/examples/#webgl_animation_cloth)






