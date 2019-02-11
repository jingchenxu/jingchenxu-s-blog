# 弹出框实现

> 这里所说的弹出框也就是我们常见的modal弹出框，主要使用到的css属性是 position、z-inde

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>modal</title>
    <style type="text/css">
    .modal-mask {
      position: fixed;
      z-index: 1000;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      background-color: rgba(0, 0, 0, 0.3)
    }
    .modal-content {
      position: fixed;
      top: 0px;
      right: 0px;
      z-index: 9000;
      width: 100px;
      height: 100px;
      background-color: red;
    }
    </style>
</head>
<body>
    <div class="">
    <button id="button"></button>
    <div class="modal-mask">
      <div class="modal-content">

      </div>
    </div>
    </div>
</body>
<script>
window.onload = () => {
  let button = document.getElementById('button')
  console.dir(button)
}
</script>
</html>
````