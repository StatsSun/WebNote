# JavaScript.Note

一、贷款计算器

1. JS代码

```javascript
function calculate() {
    var amount = document.getElementById("amount"),
        apr = document.getElementById("apr"),
        years = document.getElementById("years"),
        zipcode = document.getElementById("zipcode"),
        payment = document.getElementById("payment"),
        total = document.getElementById("total"),
        totalinterest = document.getElementById("totalinterest");

    var principal = parseFloat(amount.value);
    var interest = parseFloat(apr.value)/100/12;
    var payments = parseFloat(years.value)*12;

    var x = Math.pow(1 + interest,payment);
    var monthly = (principal * x * interest)/(x-1);

    if(isFinite(monthly)){
        payment.innerHTML = monthly.toFixed(2);
        total.innerHTML = (monthly * payment).toFixed(2);
        totalinterest.innerHTML = ((monthly*payments)-principal).toFixed(2);

        save(amount.value,apr.value,years.value,zipcode.value);

        try {
            getlenders(amount.value,apr.value,years.value,zipcode.value);
        }
        catch(e){}
            chart(principal,interest,monthly,payments);
    }

    else{
            payment.innerHTML = "";
            total.innerHTML = "";
            totalinterest.innerHTML = "";
            chart();
        }
}

function save(amount,apr,years,zipcode) {
    if(window.localStorage){
        localStorage.loan_amount = amount;
        localStorage.loan_apr = apr;
        localStorage.loan_years = years;
        localStorage.loan_zipcode = zipcode;
    }
}

window.onload = function () {
    if(window.localStorage && localStorage.loan_amount){
        document.getElementById("amount").value = localStorage.loan_amount;
        document.getElementById("apr").value = localStorage.loan_apr;
        document.getElementById("years").value = localStorage.loan_years;
        document.getElementById("zipcode").value = localStorage.loan_zipcode;
    }
};

function getlenders(amount,apr,years,zipcode) {
    if(!window.XMLHttpRequest) return;
    var ad = document.getElementById("lenders");
    if(!ad) return;
    var url = "getlenders.php" +
        "?amt=" + encodeURIComponent(amount) +
        "?apr=" + encodeURIComponent(apr) +
        "?yrs=" + encodeURIComponent(years) +
        "?zip=" + encodeURIComponent(zipcode);

    var req = new XMLHttpRequest();
    req.open("GET",url);
    req.send(null);
    req.onreadystatechange = function () {
        if(req.readyState === 4 && req.status === 200){
            var response = req.responseText;
            var lenders = JSON.parse(response);
            var list = "";
            for(var i = 0;i<lenders.length;i++){
                list +="<li><a href = '"+lenders[i].url+"'>" +
                    lenders[i].name + "</a>>";
            }
            ad.innerHTML = "<ul>" + list + "</ul>";
        }
    }
}
```







二、复选框设置

1. HTML

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>复选框设置</title>
   </head>
   <body>
       <!---->
       <input type="button" value="全选" id="btn1"><br>
       <input type="button" value="不选" id="btn2"><br>
       <input type="button" value="反选" id="btn3"><br>
       <div id="checkbox">
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox"><br>
           <input type="checkbox">
       </div>
   </body>
   </html>
   ```

2. JS代码

```javascript
<script>
        window.onload=function () {
            var btn1 = document.getElementById("btn1");
            var btn2 = document.getElementById("btn2");
            var btn3 = document.getElementById("btn3");
            var checkbox = document.getElementById("checkbox");
            var box = checkbox.getElementsByTagName("input");

            btn1.onclick = function () {
                for(var i=0;i<box.length;i++){
                    box[i].checked = true;
                }
            };

            btn2.onclick = function () {
                for(var i=0;i<box.length;i++){
                    box[i].checked = false;
                }
            };

            btn3.onclick = function () {
                for(var i=0;i<box.length;i++){
                    if(box[i].checked === true){
                        box[i].checked = false;
                    }
                    else {
                        box[i].checked = true;
                    }
                }
            }
        }
    </script>
```



三、笔记javascript

1、js区分大小写，js有内定关键字作为标识符，设置函数名称及变量命名时避免使用

	double \ goto \ short \ long \ char \ throws \ static \ class \ float \ final \ public \ transient \ package等				等；
	
	Array \ Boolean \ Date \ Error \ JSON \ Math \ NaN \ Number \ RegExp \ String \object \ isNaN \ 		 Function \ undefined 等等；

2、js的可选分号：

	a、通常当前语句与下一行语句无法合并解析，js则在第一行后面填补分号；
	
	b、例外一：return \ break \ continue 和随后的表达式之间不能换行，否则会被解释为两个单独的语句；
	
	c、“++”和“--”作为后缀时，应和表达式同行，否则行尾将填补分号，同时“++”、“—”会作为下一行代码的前缀操作符并与之一起解析
	
	如：x
	
	      ++
	
	       y
	
	会被解析为x,++y;而非x++,y;

3、数据类型：原始类型和对象类型

	a、原始类型：数值、字符串、布尔值等；
	
	b、对象类型：数组、函数、日期类、正则类、错误类等；
	
	c、对象类都具有方法属性，可以通过“.”或者“[ ]”等调用
	
	d、js中所有数字均用浮点值表示；其中因二进制浮点数和四舍五入的准确度问题导致二进制浮点数无法精确表示类似0.1这样简单的数字，所以x = 0.3-0.2=0.0999....98  ，y = 0.2-0.1=0.1，x==y? 其中x不等与y，返回结果为false，涉及金融计算时慎用，此情况只发生在比较二者是否相等时出现
	
	e、字符串书写可拆分换行，但每行必须用反斜杠（\）结束，如果时需要另起一行，则用转义字符\n即可；字符串引号时使用，可单可双，但当js与html混用时注意两者单独统一使用标准，如JS统一用单引号，html统一用双引号:

```html
<button onclick= "alert('thank you')">Click Me</button>
```

4、文本（字符串）

	a、字符串直接量；
	
	b、转义字符：\n换行符 ,\b退格符, \t水平制表符, \v垂直制表符, \f换页符, \r回车符等；
	
	c、字符串连接，通过“+”号实现，原始类型是不变类型，不可被修改，包括字符串也是不能被修改的，类似replace（）、toUpperCase()的方法返回新字符串，原字符串本身无变化；
	
	d、JS定义RegExp（）构造函数，用来创建表示文本匹配模式的对象，即正则表达式；
	
	e、布尔值：任意JS的值都能转换为布尔值，有假值和真值区分，即转变为false的为假值，转变为true的为真值，undefined , null, 0, -0, NaN, ""空字符串转变为false；对象、数字、函数等都转变为true；
	
	f、null与undefined：是JS语言关键字，常用来描述空值，其typeof值为object，可以认为是特殊的对象，含义是“非对象”，是其自有类型的唯一成员，可以表示数字、字符串、对象是“无值”的；undefined也表示空值，但更倾向于表示未初始化的变量、对象的属性或者某元素不存在，如果函数无返回任何值，则返回undefined，和null的区别是undefined不是关键字，是预定义的全局变量，他的值就是“未定义”；“==”和两者转化为布尔值时是相等的，“===”严格判断时是不相等的，两者都不含任何属性和方法；
	
	g、全局对象：全局对象的属性是全局定义的方法？全局对象的初始属性不是保留字，但应当被当作保留字对待，初始属性有：
	
	全局变量undefined、NaN、infinity等；
	
	全局函数isNaN(), parseInt()等；
	
	全局对象Math和JSON；
	
	构造函数Date（）、Sting（）、object（）、Array（）等；
	
	在代码最顶级（即不在任何函数内的JS代码）可以用JS关键字this来引用全局变量；
	
	在浏览器窗口所有JS代码中，Window对象充当全局对象，其属性window引用其自身，可以替代this来引用全局对象，其定义了核心全局属性；
	
	h、包装对象：原始值数字、字符串、布尔值不能被修改，但拥有各自转变的方法，当调用String（）、Number（）、Boolean（）构造函数时，会创建一个临时对象，这个临时对象称为包装对象，任何修改只作用于临时的包装对象，原始值并不被修改，包装对象偶尔被用来区分字符串值和字符串包装对象、数字值和数字对象、布尔值和布尔对象，其type运算符返回值可以看出原始值和包装对象的不同
	
	i、不可变的原始值和可变的对象引用：原始值的比较时值的比较，只有两者值相等时才相等，对象因时可改变的，所以其比较时属性和元素值的全部比较，对象的比较时引用的比较，当且仅当他们他们引用同一个基对象时才相等；
	
	j、类型转换：原始值转换、数组对象转换为原始值；原始值转换为对象等（显式转换、隐式转换）；
	
	k、变量声明：全局变量和局部变量的作用域和优先级，其中函数中的局部变量具有声明提前的特性，
	
	L、作为属性的变量this、作用域链（其是理解with语句和闭包非常有用），这点注意再次深入学习；

五、表达式和运算符

	A、对象和数组的初始化表达式：或叫做对象直接量或数组直接量，数组直接量列表中的列表逗号之间的元素可以省略，但被省略的位置会被填充为undefined，并保留列表的原始长度；对象直接量初始值中若使用delete删除其中某些项元素时，对象或者列表长度不会变化，删除项位置被填充为undefined；
	
	B、函数定义表达式：
	
	C、属性访问表达式：主要是对象属性和数组的访问，用“.”或者“[]”访问对象属性或者数组内元素；在“'.'”和“[”之前的表达式总是会被首选计算，如果计算结果是null或者undefined，则表达式抛出类型错误异常，因为两个值都不包括任何属性的值；
	
	如果运算结果不是对象，JS会把其转换为对象；
	
	D、调用表达式：指调用函数或者方法的语法表示，函数若例如return返回值，那么这个值则是整个调用表达式的值，如果函数没有返回值，则调用表达式的值为undefined；
	
	E、对象创建表达式：用关键字new 创建对象，如果对象创建的表达式不需要传入任何参数给到构造函数，则对象名后到圆括号可以省略
	
	F、 运算符：
	
		操作数个数：区分一元运算符（-、+、typeof、--、++、！、）、二元运算符、三元运算符（?:）；
	
		操作类型和结果类型：受JS根据需要进行的操作数类型转换有关；
	
		左值、右值：左值表示表达式只能出现在赋值运算符左侧的值，如变量、对象、数组元素均为左值；
	
		运算符副作用：如赋值运算符，使用这个变量的属性或者表达式都会发生变化、delete元素符的副作	 	用：其删除一个属性，就像给这个属性赋值为undefined；
	
		运算符的结合性：指多个具有同样优先级的运算符表达式中的运算顺序；如一元操作符、赋值、三元条件运算符都具有从右至左的结合性；
	
		运算性质：除去运算符的结合性，JS总是严格按照从左到右的顺序来计算表达式
	
	H、算术表达式：
	
		“+”运算符：
	
		一元算术运算符：
	
	I、关系表达式
	
		相等和不等运算符：
	
		比较运算符：
	
		in运算符：
	
		instanceof运算符：​			
	
	J、逻辑运算符：
	
		逻辑于（&&）：
	
		逻辑或（||）：
	
		逻辑非（！）：
	
	K、赋值表达式：
	
		带操作的赋值运算：
	
	L、表达式计算：eval();
	
	M、其他运算符：
	
		条件运算符（？：）：第一个操作数是布尔值，如果为真，则执行第二个操作数，如果为假，则执行第三个操作数；
	
		typeof运算符：是一元运算符，可以和条件运算符结合使用、和switch语句结合等；

```javascript
(typeof value == "string")? "'"+value+"'" : value
```

		delete运算符：一元操作符，
	
		void运算符：
	
		逗号运算符：

六、语句

	A、语句语法：每条语句结尾处须需加上“；”分号；
	
		多条语句可用{}号包裹形成语句块；
	
		若一条语句只有一个分号，则形成空语句，空语句可以在一些for循环中见到；
	
	B、声明语句： var  和  function，分别定义变量和函数；
	
		其中var定义的对象无法用delete删除，函数中变量声明的语句会被提前到函数顶部，但初始化操作还		在原来位置执行；
	
		function中花括号是必须的，即使语句中只有一条语句，也需要用花括号包起来；函数声明的语句其函数名称和函数体均提前，脚本中所有函数包括嵌套的函数都会在当前上下文中其他代码之前声明
	
	C、条件语句：if、if else 、if elseif 、else、switch   case  break语句
	
	D、循环语句：while 、do／while、 for 、for   in、等
	
	E、跳转循环：break、continue、return、throw语句；
	
		break语句直接跳至语句块的结束或者终止闭合语句块的执行；
	
		continue语句在while循环中直接进入下一轮循环，在for循环中先计算increment表达式（即自增表			  达式），然后再判断循环；只能在循环体内使用
	
		return语句只能在函数体内出现，否则会报语法错误，当执行到return时，函数终止执行，并返回函数计算调用的值；
	
		throw语句抛出一个代表错误的数字，或者包含可读的错误消息字符串，通常返回Error类型和其子类型；通常于catch（e）、finally任一语句一起执行；
	
	F、with、 debugger、 use  strict 语句
	
		with语句用于临时扩展作用域链；
	
		debugger语句可以用来在代码中产生断点，然后程序会停止在断点处，然后可以使用调试器输出变量的值、检查调用戳
	
		use strict语句即语句解析为严格模式，会有一系列在严格模式下许主要的事项；



七、对象	 

	A、创建对象：用var 加花括号{}创建；用new object（）创建对象；object.create（）创建对象，是一个静态函数；
	
	B、原型对象；object对象，查找object对象中的属性及属性值；
	
	C、属性查询和设置：点号法和方括号法，点号访问对象属性时，属性名是用表示符表示的，表示符直接字JS程序中出现，他们不是数据类型，所以程序无法修改它们；方括号访问对象属性时，属性名是通过字符串表示的，字符串是JS的数据类型，程序运行时可以修改和创建它们；
	
	D、关联数组：
	
	E、继承：对象的原型属性构成一个“链”，通过这个“链”可以实现属性的继承；当新建对象添加的属性名和继承来的属性同名时，则继承属性值被覆盖，原继承对象值不变；
	
	F、属性访问错误：语法错误和类型错误
	
	H、删除属性：delete运算符只能删除自有属性，不能删除继承属性，不能删除那些可配置性为false的属性；
	
	I、检测属性：for in 、hasOwnPreperty() 、  propertyIsEnumberable()方法检测某个对象中是否含有被检测属性；其中in 可以区分值为null和undefined的属性；
	
	J、枚举属性：对象继承的内置方法不可枚举，但在代码中给予对象添加的属性可枚举；
	
		枚举方法：for   in  方法；
	
		Obiect.keys():返回数组，数组由对象中可枚举的自有属性名称构成；
	
		Obiect.getOwnpropertyNames():返回对象中所有的属性名称，包括不可枚举的属性；

	K、属性getter和setter：被称为存取器属性，不具有可写性；

		如果属性同时具有getter和setter方法，则它是一个只读／写属性（数据属性中有一部分例外）；

		读取只写读属性总是返回undefined；

		存取器属性和数据属性一样，可以继承；

	L、属性的特性：属性包含名、值、特性（value、可写、可枚举、可配置特性）；

		数据属性的4个特性是：值（value）、可读性（writable）、可枚举（enumerated）、可配置（configurable）；

		存取器属性的4个特性：读取（get） 、写入（set）、可枚举、可配置；

		获得某个对象特定属性的属性描述符：object.getOwnPropertyDescriptor()，只能到的自有属性的描述符，返回的描述符形式为：{value:1, writable:true, enumerable:true, configurable:true}、或着{get:/*func*/, set:undefined, enumerable:true, configurable:true};若想获得继承属性的特性，需要便利原型链；

		设置属性的特性：object.defineProperty(),传入要修改的对象、要创建或修改的属性名称以及属性描述符对象：

```javascript
var o = {}
//添加一个可读写、可枚举、值为1的数据属性
Object.defineProperty(o,"x",{value:1, writable:true, enumerable:true, configurable:true})

```

		对于新创建的属性来说，默认的特性值是false或undefined；

M、对象的三个属性：原型属性、类属性、可扩展性

	原则属性：Object.prototype作为对象直接量的原型；

	类属性：classof();

	可扩展性：Object.esExtensions()、Object.seal()、Object.isSealed()检测对象是否封闭，Object.frozen严格锁定冻结对象，使用Object.isfrozen()检测对象是否冻结；

N、序列化对象：JSON.stringify()、JSON.parse()用来序列化和还原对象；

O、对象方法：

	toString（）

	toLocalString()

	toJSON()

	valueOf()

八、数组

	A、创建数组：数据直接量或则new array（），数组具有length属性，并会自动扩展，如果通过函数方法设定length的值，则会对数组进行截断操作；

	B、数组读写：用[]方括号对数组进行读写操作，所有的数组都是对象，所有的索引都是属性名，但只有0-2的32次方-2之间的属性名才称为索引；

	C、稀疏数组：包含从0开始不连续索引的数组，逗号间隔之间的元素为undefined，其仍作为length的值计算，即数组中找不到一个元素的索引值大于或等于length值；

	D、数组元素的添加和删除：

		最常见的添加方法是：[]用针对方括号索引项赋值；

		push（）方法在数组末尾增加一个或多个元素，即从数组的尾部压入一个或多个元素给到数组；

		pop（）方法从数组尾部每次减少数组长度1，并返回被删除的元素，即改变数组的length值；

		unshift（）方法在数组首部插入一个元素，并将其他元素索引值依次增加；

		shift（）从头部删除一个元素，并将其他元素的索引值依次减小，改变数组的length值；

		delete运算符删除元素，删除元素后该位置值变为undefined，数组的length值不变，即数组变为稀疏	数组；

E、数组遍历：比较常用的方法for循环

```javascript
//获得o对象属性名组成的数组
var keys = Object.keys(o);
var values = [];
for(var i=0;i<keys.length;i++ ){
    //获得索引处的建值
    var key = keys[i];
    //保存属性值
    values[i] = o[key];
}
//1、获得对象O的属性名称组成的数组keys（为后续获得对象属性名对应的建值做准备）；
//2、遍历keys中的属性名到key中；
//3、将O[属性名]对应的值赋值到values数组中；
```

以上遍历方法每次循环都会查询数组的长度，可优化为只查询一次,方法如下：

```javascript
for(var i=0,len=keys.length;i<len;i++){
    //循环体不变
}
//for循环中可以定义多个变量，利用len变量得到恒定keys的长度，每次循环只查询一次；
```

F、多维数组：js不支持真正的多维数组，但可以两次使用[ ]操作符来近似多维数组；

```javascript
// 表格有10行
var table = new Array(10);
//每行有10列
for(var i=0; i<table.length; i++) table[i] = new Array(10);
//初始化数组
for(var row=0; row<table.length; row++){
    for(col=0; col<table[row].length; col++){
        table[row][col] = row*col;
    }
}
//执行
var product = table[5][7]     //返回35
```

		

G、数组方法：

	join（）：将所有元素转换为字符串并返回连接在一起后的字符串，可以指定可选的字符串，用来分隔数组的元素个数；

	revers（）：将数组中的元素颠倒顺序，返回逆序数组；该方法采用替换，即不通过重新排列元素产生新数组，而是在原先的数组中重新排列元素，即其可改写数组；

	sort（）：Array.sort()方法将数组中元素排序后返回排序后的数组；当不带参数是默认以字母表顺序排，若有undefined值，则拍到最后；若不安默认规则排序，则必须给sort方法传入一个比较函数，该函数决定排好序的二个参数的前后位置；

```javascript
var a = [33, 4, 111, 222];
//按数值排序
a.sort();
//负值（a<b），0(a=b)，正值(a>b)
a.sort(function(a,b){
    //
    return a-b;  
    //return b-a;  
})
```

	concat()：Array.concat() 方法创建并返回一个新数组，返回的数组包括调用concat（）的原始数组元素和concat方法中的参数，若参数中任何一个自身是数组，则连接的是数组的元素而不是数组本身；其不会修改元数组

```javascript
var a = [1, 2, 3, 4]
a.concat(4, 5);  //返回[1,2,3,4,5]
a.concat([4,5]);  //返回[1,2,3,4,5]
a.concat(4, 5, [6,7]);  //返回[1,2,3,4,5,6,7]
```



	slice()：Array.slice（）方法返回指定数组的一个片段或者子数组，两个参数指定片段或子数组的开始和结束位置；返回的数组包括第一个参数位置元素到第二个参数指定位置之前的元素，不包括第二参数所在元素；可用负值来逆序选择；其不会修改原数组

 ```javascript
var a = [1,2,3,4,5]
a.slice(0,3);  //[1,2,3]
a.slice(3)  //[4,5]
a.slice(1,-1);  //[2,3,4]
a.slice(-3,-2);  //[3]
 ```



	splice()：Array.splice（）是在数组中插入或者删除元素的通用方法，其会修改元数组；方法中第一个参数指定插入或者删除的起点，第二个参数指定应该从数组中删除的元素个数，如果省略第二个参数，则从第一个参数开始位置到数组结尾的全部元素将全部被删除；紧随其后的任意多个参数指定需要插入到数组中的元素，插入位置从第一个参数位置开始；splice返回一个由删除元素组成的数组，或者没有删除元素的空数组；

```javascript
var a = [1,2,3,4,5,6,7]
a.splice(4);  //返回[5，6，7] a变为[1,2,3,4]
a.splice(1,2);  //返回[2,3] a变为[1,4]
a.splice(1,1);  //返回[4] a变为[1]

var a = [1,2,3,4,5,6]
a.splice(2,0,"a","b");  //返回[] a变为[1,2,a,b,3,4,5,6]
a.splice(2,2,[1,2],3);  //返回[a,b] a变为[1,2,[1,2],3,3,4,5,6]
```



push()和pop():可以当作栈来使用。push方法在数组的尾部插入一个或者多个元素，并返回数组新的长度；pop方法删除数组尾部的1个元素，减小数组的长度，并返回它删除的值；⚠️这两种方法都直接修改原数组



unshift()和shift()：unshift在数组开头添加1个或者多个元素，并将已存在元素索引移向更高位置，最后返回数组新长度；shift在数组开头删除一个元素，并将已存在元素下移一个位置，最后返回删除的元素，减少数组的长度；⚠️两种方法会直接修改原数组！



toString()和toLocaleString()：将数组中每个元素转化为字符串，并输出由逗号连接的字符串列表，⚠️数组中的数组（即方括号）或其他任何新式的包裹函数形式都不会输出，只输出转化后的字符串元素；

```javascript
[1,2,3].toString(); // 返回'1,2,3'
['a','b','c'].toString(); //返回'a,b,c'
[1,[2,'c']].toString(); //返回'1,2,c'
```



H、ES5中的方法（这些方法的相同处是参数是函数形式）

forEach（）：从头到位遍历数组，为每个元素调用指定函数；其使用三个参数调用函数：数组元素、数组索引、数组本身；该函数在将所有元素遍历完前无法停止，即不能使用break等运算符，但可以使用try快中、使其抛出foreach.break异常，循环就会停止；

 ```javascript
function foreach(a,f,t){
    try{a.forEach(f,t);}
    catch(e){
        if(e===foreach.break) return;
        else throw e;
    }
}
foreach.break = new Error("stopIteration");
 ```



map():将调用的数组的每个元素传递给指定函数，并返回一个数组，它包含该函数的返回值;⚠️传递给map的函数应该有返回值，注意map返回的是新数组，它不改变调用的数组；

```javascript
var a = [1,2,3];
var b = a.map(function(x){return x*x;})
//[1,4,9]
```



filiter()：返回调用数组元素的一个子集；传递的函数是用来逻辑判定的，该函数返回true或false，如果是true则该元素加入到返回数组中来，此元素即为filleter返回子集数组元素之一；

```javascript
a = [5,4,3,2,1];
samllvalues = a.filiter(function(x){return x<3});//[2,1]
```



every()和some()：是数组的逻辑判定，应用指定函数进行判定，返回true或false；

```javascript
//every()当且仅当针对数组中所有元素调用判定函数都返回true时，它才返回true；
//当数组中检查到第一个为false时，该方法即停止检查，并返回false，否则继续检查，直到遍历判断所有元素；
 var a = [1,2,3,4,5];
a.every(function(x){return x<6});//true

//some()数组元素中至少有一个调用判定函数返回true时，即返回true，否则继续检查；
//当检查到数组中第一个元素为true时，方法即停止检查，并返回true；

```



reduce()和reduceRight()：使用指定函数将数组元素进行组合；

```javascript

```



indexOf()和lastindexOf()：搜索整个数组中具有给定值当元素，返回找到的第一个元素的索引或者如果没有找到则返回-1；

```javascript

```











	

	