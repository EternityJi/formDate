## 方法1  

```
原生的方法:
   用ajax：
 //思路: 在前台获取文件域中的图片文件 然后将此作为参数 利用XHR对象，作为参数发送给后台 请求后台返回 图片的转存过的格式
 //1.前台
   <body>
  <input type="file" id ="file">
  <div></div>
  <script>
    //获取文件域对象
     var fileInp = document.querySelector("#file");
     //获取div 盒子对象
     var div = document.querySelector("div");
     //onchange 事件  只要内容改变 就会触发该事件
      fileInp.onchange = function(){
         //fileInp是一个对象  里面包含了 files伪数组
         //console.dir( fileInp. files[0]);
         // 创建一个XMLHttpRequest对象
        var xhr = new XMLHttpRequest();
         xhr.open("post", "file.php");
         //创建formDate
         var formDate = new FormData();
         //给formDate添加参数
         formDate.append("file",fileInp. files[0]);
         //发送请求体
         xhr.send( formDate);
         //响应监听
       xhr.onreadystatechange= function(){
        if(xhr.readyState == 4){

          if(xhr.status == 200){

            console.log(xhr.responseText);
            var imgURL = xhr.responseText;
            //给div 设置img 内容
             div.innerHTML = '<img src="' +  imgURL+ '">';
          }
        }

      }

      }

     
  </script>
</body>


//2.后台
  // 转存文件的步骤
  // 1. 通过 name 获取 $file
  // 2. 判断 error
  // 3. 动态生成新的文件名 (截取后缀名)
  // 4. 转存文件 move_uploaded_file( 临时文件路径, 新的文件路径 )

   $file = $_FILES['file'];
  if ( $file['error'] === 0 ) {
    $ext = strrchr( $file['name'], '.' ); // 后缀名
    $newName = time().rand(1000, 9999).$ext; // 新的文件名
    $temp = $file['tmp_name']; // 临时文件路径
    $newFileUrl = "./uploads/" . $newName; // 新的文件路径
    move_uploaded_file( $temp, $newFileUrl ); // 转存文件
    echo $newFileUrl;  // 输出文件路径
  }
```

## 方法二

```
不用通过ajax
 $('fileInp').on('change',function(){
      //1.获取选中的文件
      //files  是一个伪数组 只是一个文件  不是一个地址
      // 2.所以需要获取地址
      var file = this.files[0];
      //可以直接获取文件路径的地址
      var url = URL.createObjectURL(file);
      // 3.将文件的地址给src
      $('#img').attr('src',url).show();
   })
```

