1. Three.js的几大要素
  - 场景
    - 模型
    - 光源
  - 相机
  - 渲染器

2. requestAnimationFrame  
执行的时间大约是每秒60次  
但具体的执行时间根据rAF中的函数执行时间来决定的  
如果连续调用多个rAF函数，需要注意他们的不同rAF中函数的执行时间，导致的FPS不一致的问题。
可以通过执行前和执行后的时间进行对比。

3. 鼠标操作三维度场景  
OrbitControls.js控件支持鼠标左中右键操作和键盘方向键操作
``` javaScript
function render() {
  renderer.render(scene,camera);//执行渲染操作
}
render();
var controls = new THREE.OrbitControls(camera,renderer.domElement);//创建控件对象
controls.addEventListener('change', render);//监听鼠标、键盘事件
```
如果threejs代码中通过requestAnimationFrame()实现渲染器渲染方法render()的周期性调用，当通过OrbitControls操作改变相机状态的时候，没必要在通过controls.addEventListener('change', render)监听鼠标事件调用渲染函数，因为requestAnimationFrame()就会不停的调用渲染函数。

- tips: 注意开发中不要同时使用requestAnimationFrame()或controls.addEventListener('change', render)调用同一个函数，这样会冲突。
- 问：我可以同事在rAF和controls鼠标操作的控件中去调用同一个函数么

4. 材质效果  
比如实现玻璃效果要设置材质透明度，一些光亮的表面要添加高光效果。  
- 半透明效果
``` javascript
var sphereMaterial=new THREE.MeshLambertMaterial({
    color:0xff0000,
    opacity:0.7,
    transparent:true
});//材质对象
// wireframe	将几何图形渲染为线框。 默认值为false
```
- 添加高光效果  
``` javascript
var sphereMaterial=new THREE.MeshPhongMaterial({
    color:0x0000ff,
    specular:0x4488ee,  // 表示球体网格模型的高光颜色, 表示球体网格模型的高光颜色
    shininess:12  // 属性可以理解为光照强度的系数
});//材质对象

// 材质类型	功能
// MeshBasicMaterial	基础网格材质，不受光照影响的材质
// MeshLambertMaterial	Lambert网格材质，与光照有反应，漫反射
// MeshPhongMaterial	高光Phong材质,与光照有反应
// MeshStandardMaterial	PBR物理材质，相比较高光Phong材质可以更好的模拟金属、玻璃等效果
```

5. 光源  
``` javascript
// 光源	简介
// AmbientLight	环境光
// PointLight	点光源  可以插入多个点光源
// DirectionalLight	平行光，比如太阳光
// SpotLight	聚光源

//环境光    环境光颜色与网格模型的颜色进行RGB进行乘法运算
var ambient = new THREE.AmbientLight(0x444444);
scene.add(ambient);

//点光源
var point = new THREE.PointLight(0xffffff);
point.position.set(400, 200, 300); //点光源位置
// 通过add方法插入场景中，不插入的话，渲染的时候不会获取光源的信息进行光照计算
scene.add(point); //点光源添加到场景中
```
- 立体效果
> 仅仅使用环境光的情况下，你会发现整个立方体没有任何棱角感，这是因为环境光只是设置整个空间的明暗效果。如果需要立方体渲染要想有立体效果，需要使用具有方向性的点光源、平行光源等。
- 光源光照强度
> 通过光源构造函数的参数可以设置光源的颜色，一般设置明暗程度不同的白光RGB三个分量值是一样的。如果把THREE.AmbientLight(0x444444);的光照参数0x444444改为0xffffff,你会发现场景中的立方体渲染效果更明亮。

7. 几何体
``` javaScript
//长方体 参数：长，宽，高
var geometry = new THREE.BoxGeometry(100, 100, 100);
// 球体 参数：半径60  经纬度细分数40,40
var geometry = new THREE.SphereGeometry(60, 40, 40);
// 圆柱  参数：圆柱面顶部、底部直径50,50   高度100  圆周分段数
var geometry = new THREE.CylinderGeometry( 50, 50, 100, 25 );
// 正八面体
var geometry = new THREE.OctahedronGeometry(50);
// 正十二面体
var geometry = new THREE.DodecahedronGeometry(50);
// 正二十面体
var geometry = new THREE.IcosahedronGeometry(50);
```
如果有多个几何体需要加入场景新建一个材质对象后使用add方法加入场景中
``` javascript
// 立方体网格模型
var geometry1 = new THREE.BoxGeometry(100, 100, 100);
var material1 = new THREE.MeshLambertMaterial({
  color: 0x0000ff
}); //材质对象Material
var mesh1 = new THREE.Mesh(geometry1, material1); //网格模型对象Mesh
scene.add(mesh1); //网格模型添加到场景中

// 圆柱网格模型
var geometry3 = new THREE.CylinderGeometry(50, 50, 100, 25);
var material3 = new THREE.MeshLambertMaterial({
  color: 0xffff00
});
var mesh3 = new THREE.Mesh(geometry3, material3); //网格模型对象Mesh
// mesh3.translateX(120); //球体网格模型沿Y轴正方向平移120
mesh3.position.set(120,0,0);//设置mesh3模型对象的xyz坐标为120,0,0
scene.add(mesh3); //
```
8. 辅助三维坐标系AxisHelper  
为了方便调试预览threejs提供了一个辅助三维坐标系AxisHelper，可以直接调用THREE.AxisHelper创建一个三维坐标系，然后通过.add()方法插入到场景中即可。
``` javascript
// 辅助坐标系  参数250表示坐标系大小，可以根据场景大小去设置
var axisHelper = new THREE.AxisHelper(250);
scene.add(axisHelper);
```


导入3d格式的模型  
OBJLoader, 无法包含动画，可以包含材质和颜色，没有特别大的动画需求
Fbx    动画要求大
glt    步骤很麻烦

注意事项  
单位统一  
命名规则  
导入后配置灯光  
three.js场景配置要素 ：  
scene、模型object3d、材质material、灯光light、相机camera、渲染renderer(webglRender,css3dRenderer)

导入后黑屏可能是没有加光元素或者相机位置不对导致的黑屏。

