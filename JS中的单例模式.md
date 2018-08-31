# 单例模式

- 怎么写

  ```JavaScript
  //使用闭包实现单例模式  
  var gameInstance=(function(){  
    
      var inst=null;  
    
      function getInstance(){  
          if(inst ==null){  
    
              inst = new myobj();  
              console.log("new instance");  
              return inst;  
          }else{  
              return inst;  
          }  
      }  
    
    
      function myobj(){  
    
          this.name="zjw";  
          this.age=24;  
      }  
    
      myobj.prototype.show = function(){  
          console.log("name:"+this.name+" age:"+this.age);  
      };  
    
    
      return getInstance;  
  })();  
    
    
  gameInstance().show();  
  gameInstance().show();  
    
    
  //结果：  
    
  new instance  
  name:zjw age:24  
  name:zjw age:24  
  ```

  ​