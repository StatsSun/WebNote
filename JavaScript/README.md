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



