### 一、改变 this 指向

1. bind 改变 this 指向 bind({}||[])。bind 后形成新的函数，需要调用

   ```
   function myFn(){
   	console.log(this)
   }
   const obj = {
   	name:'xiaohong',
   	age:8
   }

   const newFn =  myFn.bind(obj)
   newFn()// {
               name:'xiaohong',
               age:8
             }
   ```

2. call 无需调用,call(obj,'1','2')

   ```
   var Person = {
       name: "zhangsan",
       age: 19
   }

   function aa(x, y) {
       console.log(x + "," + y);
       console.log(this);
       console.log(this.name);

   }

   aa(4, 5); //this指向window   4,5  window  空

   aa.call(Person, 4, 5); //this指向Person   4,5,Person{}对象 ,zhangsan
   ```

3. Apply 与 call 相似 无需调用，参数为数组格式 apply(obj,[])

   ```
   var Person = {
       name: "zhangsan",
       age: 19
   }

   function aa(x, y) {
       console.log(x + "," + y);
       console.log(this);
       console.log(this.name);
   }

   aa.apply(Person, [4, 5]); //this指向Person   4,5，Person{}对象 ,zhangsan
   ```

### 二、this 谁调用 this 指向谁

1. **函数内嵌套函数**

   ```javascript
   function fire() {
     // 我是被定义在函数内部的函数哦！
     function innerFire() {
       console.log(this === window); //未明确调用对象，this指向window
     }
     innerFire(); // 独立函数调用
   }
   fire(); // 输出true window.fire,this=window
   innerFire(); //报错
   ```

2. **对象内层函数内部函数**

   ```
   var obj = {
       fire: function () { //此时的fire函数其实用到了隐式绑定
       console.log(this,'99999')
           function innerFire() {
               console.log(this === window); 		//未明确调用对象，this指向window
           }
           innerFire();
       }
   }
   obj.fire(); //输出 true
   //分析
   	console.log(this,'99999'),this = obj,原因obj调用了fire(),this指向obj
   	函数内部调用innerFire没有明确调用对象，默认指向window
   ```

3. #### this 隐式绑定

   1. **隐士绑定：**fire 函数并不会因为它被定义在 obj 对象的内部和外部而有任何区别，

   ```
   var obj = {
       a: 1,
       fire: function () { //此时函数的this被隐式绑定到了对象obj
           console.log(this == obj); // obj中有fire函数，因而默认this指向obj
           console.log(this.a); // 1 this.a 相当于obj.a =1
       }
   }
   obj.fire(); // 输出

    相同的方法： fire函数并不会因为它被定义在obj对象的内部和外部而有任何区别，
    ============
   function fire() {
       console.log(this.a)
   }

   var obj = {
       a: 1,
       fire: fire
   }
   obj.fire(); // 输出1

   ```

   2. **动态绑定：** 实在代码运行期绑定而不是在书写期

      ```
      var a = 2;
      var obj = {
          a: 1, // a是定义在对象obj中的属性
          fire: function () {
              console.log(this.a)
          }
      }

      function otherFire(fn) { //全局函数，this绑定window
          fn();
      }
      otherFire(obj.fire); // 输出2   this对于obj的绑定丢失，绑定到了全局this上面   window.otherFire(),this = window
      =================
      var obj = {
          a: 1,
          obj2: {
              a: 2,
              obj3: {
                  a: 3,
                  getA: function () { //obj3.getA()   this绑定到了obj3当中
                      console.log(this.a)
                  }
              }
          }
      }
      obj.obj2.obj3.getA(); // 输出3 由左向右
      ```
