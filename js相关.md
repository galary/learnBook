## 手写promise.all
1.参数为promise对象数组

2.返回的是一个promise对象,并且resolve()传递一个promise对象数组给之后的then操作

3.当全部返回时才进行使用,也就是conut === promiseArray.length是进行resolve(reslut)操作

4.必须是一个回调,因为每个请求的响应事件不一样,有长有短,所以需要使用回调进行’未来时’操作,进行计数

 ```ruby 
Promise.all1 = function (promises) {
  let results = new Array();
  return new Promise(async function (resolve, reject) {
    promises.forEach(promise => {
      promise.then(res => {
        results.push(res);
        if (results.length === promises.length) {
          resolve(results);
        }
      });
    });
  });
}
```
