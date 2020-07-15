### this
1. JavaScript 中的 this 是什么  
this 是和执行上下文绑定的，也就是说每个执行上下文中都有一个 this

2. 全局执行上下文中的 this  
全局执行上下文中的 this 是指向 window 对象的

3. 函数执行上下文中的 this  
	- 通过函数的 call 方法设置
	- 通过对象调用方法设置  
	使用对象来调用其内部的一个方法，该方法的 this 是指向对象本身的。  
	在全局环境中调用一个函数，函数内部的 this 指向的是全局变量 window。
	- 通过构造函数中设置  
	(```)
	function CreateObj(){
	  this.name = "极客时间"
	}
	var myObj = new CreateObj()
	(```)
	首先创建了一个空对象 tempObj；  
	接着调用 CreateObj.call 方法，并将 tempObj 作为 call 方法的参数，这样当 CreateObj 的执行上下文创建时，它的 this 就指向了 tempObj 对象；  
	然后执行 CreateObj 函数，此时的 CreateObj 函数执行上下文中的 this 指向了 tempObj 对象；  
	最后返回 tempObj 对象。

4. this 的设计缺陷以及应对方案
	- 嵌套函数中的 this 不会从外层函数中继承  
	声明一个变量 self 用来保存 this  
	也可以使用 ES6 中的箭头函数来解决这个问题  
	第一种是把 this 保存为一个 self 变量，再利用变量的作用域机制传递给嵌套函数。  
	第二种是继续使用 this，但是要把嵌套函数改为箭头函数，因为箭头函数没有自己的执行上下文，所以它会继承调用函数中的 this。
	- 普通函数中的 this 默认指向全局对象 window  
	如果要让函数执行上下文中的 this 指向某个对象，最好的方式是通过 call 方法来显示调用。  
	在严格模式下，默认执行一个函数，其函数的执行上下文中的 this 值是 undefined，这就解决上面的问题了。


总结：  
1. 当函数作为对象的方法调用时，函数中的 this 就是该对象；
2. 当函数被正常调用时，在严格模式下，this 值是 undefined，非严格模式下 this 指向的是全局对象 window；
3. 嵌套函数中的 this 不会继承外层函数的 this 值。
4. 因为箭头函数没有自己的执行上下文，所以箭头函数的 this 就是它外层函数的 this。this始终指向函数声明时所在的词法作用域下this的值。
5.箭头函数不能作为构造函数去实例化对象； 箭头函数当中不能使用arguments；
箭头函数适合于this无关的回调。定时器，数组的方法回调
箭头函数不适合于this有关的回调。事件回调，对象的方法




