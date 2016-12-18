## 6.2 创建对象
1. 工厂模式
    ```javascript
    function createPerson(name,age,job){
        var o = new Object()
        o.name = name
        o.age = age
        o.job = job
    }
    ```
    - 缺点：无法得知所创建的对象的类型
2. 构造函数模式
    ```javascript
    function Person(name,age,job){
        this.name = name
        this.age = age
        this.job = job
        this.sayName = function(){
            alert(this.name)
        }
    }
    var person1 = new Person('a',20,'Software Engineer');
    var person2 = new Person('b',40,'Doctor');
    person1.constructor === person2.constructor
    ```
    - 优点：构造函数可以将它的实例标识为一种特定类型
    - 缺点：需要为内部方法创建不同Function实例
3. 原型模式
    ```javascript
    function Person(){}
    Person.prototype.name = 'Nicholas'
    Person.prototype.age = 29
    Person.prototype.job = 'Software Engineer'
    Person.prototype.sayName = function(){
        alert(this.name)
    }
    var person1 = new Person()
    var person2 = new Person()
    person1.sayName === person2.sayName
    ```
    - tips: 
        - hasOwnProperty可用来检测属性是否属于实例，in操作符不作区分
        - delete可删除实例属性
        - Object.keys只返回当前属性（不向上读），Object.getOwnPropertyNames()同前，不过会返回不可枚举属性。
        - 重写原型会切断现有原型和之前所有实例之间的联系
    - 优点：所有实例共享原型属性和方法
    - 缺点：实例中可直接修改原型中引用类型的值，导致所有实例此值均被改变
4. 组合使用构造函数和原型模式
    ```javascript
    function Person(name,age,job){
        this.name = name
        this.age = age
        this.job = job
        this.friends = ['Shelby','Court']
    }
    Person.prototype = {
        constructor: Person,
        sayName: function(){
            alert(this.name)
        }
    }
    ```
    特点：ECMAScript中使用最广泛，认同度最高的一种创建自定义类型的方法。这是定义引用类型的一种默认模式。
5. 动态原型模式
    ```javascript
    function Person(name,age,job){
        this.name = name
        this.age = age
        this.job = job
        if(typeof this.sayName != "function"){
            Person.prototype.sayName = function(){
                console.log(this.name)
            }
        }
    }
    