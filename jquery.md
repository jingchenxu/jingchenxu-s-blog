## e.target && e.currentTarget

在jQuery中我们常常会使用这样的代码：

````javascript
var addressList = $(".address-item");
//为每个item添加监听事件
function choiceAddress(e) {
  //标注位置
  var index = e.currentTarget.getAttribute('data-index');
  console.log(index);
  for (var i = 0; i < addressList.length; i++) {
    var address = addressList[i];
    var _index = address.getAttribute('data-index');
    if (_index == index) {
      $(address).removeClass("address-active");
      $(address).addClass("address-active");
    }
    else {
      $(address).removeClass("address-active");
    }
  }
}
for (var i = 0; i < addressList.length; i++) {
  var addressItem = addressList[i];
  $(addressItem).bind("click", choiceAddress);
}
````

在上面代码的标注位置，如果我们使用的是e.target,会发现我们去到的元素对象不是我们添加事件监听时的那个对象，因为e.target指的是收到点击的那个对象，而使用e.currentTarget则是当初设置监听的那个对象。
