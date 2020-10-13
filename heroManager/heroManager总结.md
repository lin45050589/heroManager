## heroManager 总结

## 技术栈：

+ 图片预览：固定四步

  ```js
  //1.给file表单元素注册onchange事件
  $('file表单').change(function () {
    //1.2 获取用户选择的图片
    var file = this.files[0];
    //1.3 将文件转为src路径
    var url = URL.createObjectURL(file);
    //1.4 将url路径赋值给img标签的src
    $('img元素').attr('src', url);
  });
  ```

  

+ 表单提交 

  ```js 
        $('提交按钮').on('click',function(e){
          //禁用表单默认提交事件
          e.preventDefault();
          //创建FormData对象：参数是表单dom对象
          var fd = new FormData('form表单DOM对象')
          $.ajax({
            url:'',
            type:'post',
            dataType:'json',
            data:fd,
            contentType: false,
            processData: false,
            success: function(backData){
            }
          });
        });
  ```

  

+ ajax请求数据

  ```js 
        $.ajax({
          url:'',
          type:'get',
          dataType:'json',
          data:'',
          success: function(backData){
          }
        });
  ```

+    模板引擎的使用

     ```js
     //第一步：导包
      <script src="./lib/js/template-web.js"></script>
     //第二步：创建模板引擎
     <script id=heroList type="tetx/html">模板引擎</script>
     //第三步：挂载到页面
      $('挂载点').html(template('模板引擎的id',backData))
     ```

     

## login 页面
+ 技术栈： 思路分析：
    - 点击登录按钮注册点击事件

      ````js
      $('点击按钮').click(function(e){})
      ````

    - 阻止表单的默认跳转事件

      ````js
        e.preventDefault(); 
      ````

    - 获取用户名和密码

      ```js
      let username=$('用户名').val().trim()；
      let parssword=$('密码').val().trim();
      ```

    - 非空判断

      ```js
      if(username==''||parssword==''){
          alter('用户名和账户不能为空');
          return;
      }
      ```

    - 发送ajax请求

      ```js
      $.ajax({
       url:'url接口地址'，
       type:'post/get',
       data:'需要参数'，
          success：function(backData){
          console.log(backData);
          //渲染页面
          
          ....
          
      }
      })
      ```

      

    - 渲染页面

      ```js
      //判断返回数据的msg
      if(backData.code==200){
          alter(backData.msg)
          
      }
      ```

      

    - 如果成功跳转到首页

      ```js
      window.loction.href='跳转页面'
      ```

      

    - 失败提供错误信息

      ```js 
      alter(backData.mag)
      ```

    - 登录页面完整 js代码

      ```js 
          $(function () {
            $('.login').click(function(e){
              // 阻止表单的默认提交
              e.preventDefault();
              // 获取用户输入
              let userName= $('#username').val().trim();
              let password= $('#password').val().trim();
      
              // console.log('userName',userName,'password',password);
              // 非空判断
              if(userName==''||password==''){
                alert('账号和密码不能为空');
                return;
              }
              // 发送请求
              $.ajax({
                url:'http://localhost:4399/user/login',
                type:'post',
                dataType:'json',
                data:{
                  username:userName,
                  password:password
                },
                success: function(backData){
              console.log(backData);
              if(backData.code==200){
                alert('登录成功')
                window.location.href="./index.html";
      
              }else{
                alert(backData.msg)
              }
                }
              });
            })
          })
      
      ```

## 首页 index 
+ 技术栈：思路
    - 页面一加载，发送ajax，渲染数据

      ```js
      functuon getData()(
          $.ajax({
              url:'所有英雄数据接口'，
              type：'get/post',,
              data:'',
              success:function(backData){
              console.log(backData)
              //渲染数据
              $('挂载点').html(template('模板引擎的id',backData))
          }
          })
      )
      //调用
      getData()；
      ```

    - 使用模板引擎的步骤：

      ```html
      //第一步：导包
       <script src="./lib/js/template-web.js"></script>
      //第二步：创建模板引擎
      <script id=heroList type="tetx/html">模板引擎</script>
      //第三步：挂载到页面
       $('挂载点').html(template('模板引擎的id',backData))
      ```

      

    - 使用模板引擎—渲染从服务器返回的数据

      ```html
      <script id=heroList type="tetx/html">
      {{each data v}}
      	   <tr >
            <td><img src="{{v.icon}}" alt=""></td>
            <td>{{v.name}}</td>
            <td>{{v.skill}}</td>
            <td>
              <button onclick="location.href='./edit.html?id={{v.id}}'" class="btn btn-primary"  >编辑</button>
              <button onclick="alert('你真狠')" class="btn btn-danger delete" data-id='{{v.id}}'>删除</button>
            </td>
          </tr>
      {{/each}}
      </script>
      ```

      

    - 点击删除按钮，注册点击事件

    - 注意点： 删除按钮是动态生成的

      ```js
      $("删除按钮的父元素").on('click","删除按钮" , function(){
       //函数体
       }
      ```

    - 提示用户是否要删除 —confirm 判断结果为true 是确定删除，flase 是取消删除

      ```js
      if(confirm(确认要删除吗)){
          //确认删除需要处理执行的代码
      }
      ```

    - 获取当前删除数据的id

      ```js
      let id=$(this).attr('data-id')
      ```

    - 发s送ajax请求

      ```js
      $.ajax({
          url:'删除数据接口'，
          type:'get',
          dataType:'json,
          data:{
          id:id,
      },sucess:function(backDat){
          console.log(backData)
          //渲染服务器返回的数据
      }
      })
      ```

      

    - 渲染服务器返回的数据有两种结果

    - 判断是否删除成功 ，有两种结果  成功或不成功

      ```js
      
        if(backData.code==204){
            alert('删除成功')
            //删除成功的代码
            
        }
      
      ```

      

    - 如果删除成功，需要重新加载数据

      * 载数据的方式有三种

      * 重新加载数据

        ```js 
        window.location.reload();
        ```

      * 局部刷新 : 调用页面一加载发送ajax函数

      * ```js
        getData()
        ```

      * 只删除当前选择行

        ```js
        //需要在发送ajax之前保存当前点击的this
        let _that.=$(this);
        //只删除当前点击的行
        _that.parent().parent().remove();
        ```

      * 

    - 删除按钮完整的js代码

      ```js
        $('#my-table>tbody').on('click', '.delete', function () {
              if (confirm('确定要删除吗')) {
                let _that = $(this);
                let id = $(this).attr('data-id');
                $.ajax({
                  url: 'http://localhost:4399/hero/delete',
                  type: 'get',
                  dataType: 'json',
                  data: {
                    id: id
                  },
                  success: function (backData) {
                    console.log(backData);
                    if (backData.code == 204) {
                      alert(backData.msg);
                      // // 全局刷新
                      // window.location.reload();
                      // // 局部刷新
                      // getData();
                      // 只删除当前行
                      _that.parent().parent().remove();
                    }
                  }
                });
              }
            })
          })
      ```

    - 新增按钮—设置a标签的href属性

      ```html
       <a href="./add.html" class="btn btn-success pull-right">新增</a>
      ```

      

    - 编辑按钮—设置编辑的点击跳转到编辑页面

      ```html
       <button onclick="location.href='./edit.html'" class="btn btn-primary">编辑</button>
      ```

    - 首页完整js代码

      ```js
        $(function () {
            function getData() {
              $.ajax({
                url: 'http://localhost:4399/hero/all',
                type: 'get',
                dataType: 'json',
                data: '',
                success: function (backData) {
                  console.log(backData);
                  $('#my-table>tbody').html(template('heroList', backData))
                }
              });
            }
            getData();
      
            // 删除
            $('#my-table>tbody').on('click', '.delete', function () {
      
              if (confirm('确定要删除吗')) {
                let _that = $(this);
                let id = $(this).attr('data-id');
                $.ajax({
                  url: 'http://localhost:4399/hero/delete',
                  type: 'get',
                  dataType: 'json',
                  data: {
                    id: id
                  },
                  success: function (backData) {
                    console.log(backData);
                    if (backData.code == 204) {
                      alert(backData.msg);
                      // // 全局刷新
                      // window.location.reload();
                      // // 局部刷新
                      // getData();
                      // 只删除当前行
                      _that.parent().parent().remove();
                    }
                  }
                });
              }
            })
          })
      ```

    - 完整的模板引擎

      ```js
        <!-- 引入模板引擎js文件 -->
        <script src="./lib/js/template-web.js"></script>
        <!-- 准备一个模板 -->
        <script id="heroList" type="text/html">
          {{each data v}}
          <tr >
            <td><img src="{{v.icon}}" alt=""></td>
            <td>{{v.name}}</td>
            <td>{{v.skill}}</td>
            <td>
              <button onclick="location.href='./edit.html?id={{v.id}}'" class="btn btn-primary"  >编辑</button>
              <button onclick="alert('你真狠')" class="btn btn-danger delete" data-id='{{v.id}}'>删除</button>
            </td>
          </tr>
          {{/each}}
        </script>
        
      ```

      

      

## 新增页面

+ 图片预览

  ```js
        // 图片预览
        //1.给file表单元素注册onchange事件
        $('#heroIcon').change(function () {
          //1.2 获取用户选择的图片
          var file = this.files[0];
          //1.3 将文件转为src路径
          var url = URL.createObjectURL(file);
          //1.4 将url路径赋值给img标签的src
          $('img.preview').attr('src', url);
        });
  ```

  

+ 表单上传

  ```js
    // 新增加文件上传
        $('.btn-add').on('click', function (e) {
          //禁用表单默认提交事件
          e.preventDefault();
          //创建FormData对象：参数是表单dom对象
          var fd = new FormData($('form')[0])
          // console.log(fd.get('icon'));
          // console.log(fd.get('name'));
          // console.log(fd.get('skill'));
  
          $.ajax({
            url: 'http://localhost:4399/hero/add',
            type: 'post',
            dataType: 'json',
            data: fd,
            contentType: false,
            processData: false,
            success: function (backData) {
              console.log(backData);
              if (backData.code==201) {
                alert(backData.msg);
                window.location.href = './index.html'
              } else {
                alert(backData.msg);
              }
            }
          });
        });
  ```

+ 新增页面完整js代码

  ```js
   $(function () {
        // 图片预览
        //1.给file表单元素注册onchange事件
        $('#heroIcon').change(function () {
          //1.2 获取用户选择的图片
          var file = this.files[0];
          //1.3 将文件转为src路径
          var url = URL.createObjectURL(file);
          //1.4 将url路径赋值给img标签的src
          $('img.preview').attr('src', url);
        });
  
  
        // 新增加文件上传
        $('.btn-add').on('click', function (e) {
          //禁用表单默认提交事件
          e.preventDefault();
          //创建FormData对象：参数是表单dom对象
          var fd = new FormData($('form')[0])
          // console.log(fd.get('icon'));
          // console.log(fd.get('name'));
          // console.log(fd.get('skill'));
  
          $.ajax({
            url: 'http://localhost:4399/hero/add',
            type: 'post',
            dataType: 'json',
            data: fd,
            contentType: false,
            processData: false,
            success: function (backData) {
              console.log(backData);
              if (backData.code==201) {
                alert(backData.msg);
                window.location.href = './index.html'
              } else {
                alert(backData.msg);
              }
            }
          });
        });
      })
  ```

  

## 编辑页面
+ 获取当前点击英雄的id

```js
let id= window.location.serch.split('=')[1]
```

+ 发送ajax请求

  ```js
  $.ajax({
      url:'http://localhost:4399/hero/id'，
      type:'get',
      dataType:'json',
      data:{
      id:id
  },sussecc:function(backData){
      conslon.log(backData)
      //获取的数据渲染到页面
      $('用户名').val(backData.data.name);
      $('英雄技能').val(backData.data.skill);
      $('英雄的头像').val(backData.data.icon)
  }
         
  })
  ```

+ 图片预览四步

  ```js
  //1、给file表当表单注册onchange事件
  $('注册表单按钮').change(function(){
      // 1.2、获取用户选择的图片
      var file=this.files[0];
  	// 1.3、 将文件转为src路径
       var url = URL.createObjectURL(file);
      // 1.4、 将文件路径赋值给img 标签
      $(this).prev(img).attr('src',url)
  })
  ```

  

+ 注册表单click事件

  ```js
  $('注册表单').on('clicl',functiom(e){
      //阻止表单默认提交事件
     e.preventDefault();
  	//创建FormData对象：参数是表单dom对象
  	var fd=new FormData($('form')[0]);
  
  	fd.append('id',id);
     //发送ajax请求
  $.ajax({
      url:'http://localhost:4399/hero/update',
      type:'post',
      dataType:'json',
      data:fd,
      success:function(backData){
          console.log(backData);
          //是否修改成功
          if(backData.code==202){
              alter('修改成功')；
             //刷新页面
              window.location.href='./index/html'
              
          }else{
              alter(backData.msg)
          }
          
      }
      
  })
  
   })
  ```

+ 编辑页面完整js

  ```js
   //入口函数
      $(function () {
        //1.给file表单元素注册onchange事件
        $('#heroIcon').change(function () {
          //1.2 获取用户选择的图片
          var file = this.files[0];
          //1.3 将文件转为src路径
          var url = URL.createObjectURL(file);
          //1.4 将url路径赋值给img标签的src
          $(this).prev('img').attr('src', url);
        });
  
        let id = window.location.search.split('=')[1]
        // console.log(id);
        $.ajax({
          url: 'http://localhost:4399/hero/id',
          type: 'get',
          dataType: 'json',
          data: {
            id: id,
          },
          success: function (backData) {
            // console.log(backData);
          if(backData.code=200){
            $('#heroName').val(backData.data.name)
            $('#skillName').val(backData.data.skill)
            $('#heroIcon').prev('img').attr('src',backData.data.icon)
          }
  
          }
        });
  
  
        $('.btn-save').on('click',function(e){
          //禁用表单默认提交事件
          e.preventDefault();
          //创建FormData对象：参数是表单dom对象
          var fd = new FormData($('form')[0])
          fd.append('id',id)
  
           console.log(fd.get('id'));
           console.log(fd.get('name'));
          console.log(fd.get('skill'));
          console.log(fd.get('icon'));
          
          $.ajax({
            url:'http://localhost:4399/hero/update',
            type:'post',
            dataType:'json',
            data:fd,
            contentType: false,
            processData: false,
            success: function(backData){
              console.log(backData);
              if(backData.code==202){
                alert(backData.msg);
                window.location.href="./index.html"
              }else{
                alert(backData.msg);
              }
            }
          });
        });
      })
  ```

  