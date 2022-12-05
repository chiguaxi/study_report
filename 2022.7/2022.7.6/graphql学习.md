**graphql的构造方法**

```js
const express = require('express');
const {buildSchema} = require('graphql');
const graphqlHTTP = require('express-graphql');
//定义schema,查询和类型
const schema = buildSchema('
	type Account{
		name: String
		age: Int
		sex: String
		department: String
	}
    type Query {
         hello: String
         accountName: String
         age: Int
         account: Account
    }
')
//定义查询对应的处理器
const root = {
    hello: () => {
        return 'hello world';
    },
    accountName: () =>{
        return '张三丰';
    },
    age: () => {
        return 18;
    }
    acount: () => {
    	return {
    		name: '李四光',
    		age: 18,
    		sex: '男',
    		department: '科学院'
    	}
    }
}

const app = express();//加入express拦截所有处理方式

app.use('/graphql', graphqlHTTP({
	schema: schema,
	rootValue: root,//加入root启动处理
	graphiql: true //是否打开调试模式 true为打开
}))

app.listen(3000)
```

**基本参数类型** 

基本类型：String，Int，Float，Boolean和ID。可以在schema声明是直接使用。

[类型]代表数组，例如：[Int]代表整形数组

和js传递参数一样，小括号内定义形参，但是注意：参数需要定义类型。

！(叹号)代表参数不能为空。

```js
const express = require('express');
const {buildSchema} = require('graphql');
const graphqlHTTP = require('express-graphql');
//定义schema,查询和类型
const schema = buildSchema('
    type Account{
        name: String
        age: Int
        sex: String
        department: String
        salary(city: String): Int
    }
    type Query {
        getClassMates(classNo: Int!): [String]
    	account(username: String): Account
    }
')
//定义查询对应的处理器
const root = {
    getClassMates({ classNo}){
        const obj = {
            31: ['张三','李四','王五'],
            61: ['张大三','李大四','王大五']
        }
        return obj[classNo];
    },
    account({ username}){
        const name = username;
        const sex = 'man';
        const age = 18;
        const department = '开发部';
        const salary = ({city}) => {
            if(city === "北京" || city === "上海" || city === "广州" || city === "深圳"){
                return 10000;
            }
            else return 3000;
        }
        return {
            name,
            sex,
            age,
            department,
            salary
        }
    }
}

const app = express();//加入express拦截所有处理方式

app.use('/graphql', graphqlHTTP({
	schema: schema,
	rootValue: root,//加入root启动处理
	graphiql: true //是否打开调试模式 true为打开
}))

app.listen(3000)
```

