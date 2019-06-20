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

​	j、类型转换：原始值转换、数组对象转换为原始值；原始值转换为对象等（显式转换、隐式转换）；

​	k、变量声明：全局变量和局部变量的作用域和优先级，其中函数中的局部变量具有声明提前的特性，

​	L、作为属性的变量this、作用域链（其是理解with语句和闭包非常有用），这点注意再次深入学习；

5、表达式和运算符

​	A、对象和数组的初始化表达式：或叫做对象直接量或数组直接量，数组直接量列表中的列表逗号之间的元素可以省略，但被省略的位置会被填充为undefined，并保留列表的原始长度；对象直接量初始值中若使用delete删除其中某些项元素时，对象或者列表长度不会变化，删除项位置被填充为undefined；

​	B、函数定义表达式：

​	C、属性访问表达式：主要是对象属性和数组的访问，用“.”或者“[]”访问对象属性或者数组内元素；在“'.'”和“[”之前的表达式总是会被首选计算，如果计算结果是null或者undefined，则表达式抛出类型错误异常，因为两个值都不包括任何属性的值；

​	如果运算结果不是对象，JS会把其转换为对象；

​	D、调用表达式：指调用函数或者方法的语法表示，函数若例如return返回值，那么这个值则是整个调用表达式的值，如果函数没有返回值，则调用表达式的值为undefined；

​	E、对象创建表达式：用关键字new 创建对象，如果对象创建的表达式不需要传入任何参数给到构造函数，则对象名后到圆括号可以省略

​	F、 运算符：

​		操作数个数：区分一元运算符（-、+、typeof、--、++、！、）、二元运算符、三元运算符（?:）；

​		操作类型和结果类型：受JS根据需要进行的操作数类型转换有关；

​		左值、右值：左值表示表达式只能出现在赋值运算符左侧的值，如变量、对象、数组元素均为左值；

​		运算符副作用：如赋值运算符，使用这个变量的属性或者表达式都会发生变化、delete元素符的副作	 	用：其删除一个属性，就像给这个属性赋值为undefined；

​		运算符的结合性：指多个具有同样优先级的运算符表达式中的运算顺序；如一元操作符、赋值、三元条件运算符都具有从右至左的结合性；

​		运算性质：除去运算符的结合性，JS总是严格按照从左到右的顺序来计算表达式

​	H、算术表达式：

​		“+”运算符：

​		一元算术运算符：

​	I、关系表达式

​		相等和不等运算符：

​		比较运算符：

​		in运算符：

​		instanceof运算符：​			

​	J、逻辑运算符：

​		逻辑于（&&）：

​		逻辑或（||）：

​		逻辑非（！）：

​	K、赋值表达式：

​		带操作的赋值运算：

​	L、表达式计算：eval();

​	M、其他运算符：

​		条件运算符（？：）：第一个操作数是布尔值，如果为真，则执行第二个操作数，如果为假，则执行第三个操作数；

​		typeof运算符：是一元运算符，可以和条件运算符结合使用、和switch语句结合等；

```javascript
(typeof value == "string")? "'"+value+"'" : value
```

​		delete运算符：一元操作符，

​		void运算符：

​		逗号运算符：

6、语句

​	A、语句语法：每条语句结尾处须需加上“；”分号；

​		多条语句可用{}号包裹形成语句块；

​		若一条语句只有一个分号，则形成空语句，空语句可以在一些for循环中见到；

​	B、声明语句： var  和  function，分别定义变量和函数

​	C、

​	D、

​	E、

​	F、

​	H、

​	I、

​	









	

	