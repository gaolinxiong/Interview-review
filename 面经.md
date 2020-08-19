# Interview-review
## 面试复盘记录
### 8/17 运匠信息
#### 深拷贝方法（非json.stringify()和json.parse()）
三点考虑
1. 基本类型直接复制
2. 复杂类型判断并选择下层递归直至找到基本类型
3. 循环引用在判断是复杂类型的情况下 复制至copy_obj中 若子属性和本身相等 则直接复制循环引用
```
// 解决循环引用问题
var a = {};
a.info = a;
function deepCopy(target){
    let copyed_objs = [];//此数组解决了循环引用和相同引用的问题，它存放已经递归到的目标对象
    function _deepCopy(target){
        if((typeof target !== 'object')||!target){return target;}
        for(let i= 0 ;i < copyed_objs.length;i++){
            if(copyed_objs[i].target === target){
                return copyed_objs[i].copyTarget;
            }
        }
        let obj = {};
        if(Array.isArray(target)){
            console.log('target4', target)
            obj = [];//处理target是数组的情况
        }
        copyed_objs.push({target:target,copyTarget:obj})
        Object.keys(target).forEach(key=>{
            if(obj[key]){ return;}
            obj[key] = _deepCopy(target[key]);
        });
        return obj;
    }
    return _deepCopy(target);
}
console.log(deepCopy(a));
```

http响应状态码的含义：可参 https://www.runoob.com/http/http-status-codes.html
+ 200 ：请求成功。一般用于GET与POST请求
+ 302 ：临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI
+ 401 ：请求要求用户的身份认证
+ 500 ：服务器内部错误，无法完成请求




