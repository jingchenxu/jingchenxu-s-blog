## e.target && e.currentTarget

在jQuery中我们常常会使用这样的代码：

````javascript
var addressList = $(".address-item");
//为每个item添加监听事件
function choiceAddress(e) {
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