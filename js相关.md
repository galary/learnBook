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
## 手写发布订阅模式

1.发布者和订阅者是松散耦合的，互补关心对方，他们关注的对象是事件本身

2.发布者通过事件调度中心提供的publish方法进行时间发布操作，他们并不关心有没有人订阅

3.订阅者通过事件调度中心提供的subscribe方法进行事件订阅操作，他不关心有没有人发布事件，但是只要自己订阅的事件发生，他就做出响应

#特点

.发布订阅模式中，对于发布者Publisher和订阅者Subscriber没有特殊的约束，他们好似是匿名活动，借助事件调度中心提供的接口发布和订阅事件，互不了解对方是谁。

.松散耦合，灵活度高，常用作事件总线

.易理解，可类比于DOM事件中的dispatchEvent和addEventListener。

#缺点

.当事件类型越来越多时，难以维护，需要考虑事件命名的规范，也要防范数据流混乱。

 ```ruby 
class PubSub {
    constructor() {
        // 维护事件及订阅行为
        this.events = {}
    }
    /**
     * 注册事件订阅行为
     * @param {String} type 事件类型
     * @param {Function} cb 回调函数
     */
    subscribe(type, cb) {
        if (!this.events[type]) {
            this.events[type] = []
        }
        this.events[type].push(cb)
    }
    /**
     * 发布事件
     * @param {String} type 事件类型
     * @param  {...any} args 参数列表
     */
    publish(type, ...args) {
        if (this.events[type]) {
            this.events[type].forEach(cb => {
                cb(...args)
            })
        }
    }
    /**
     * 移除某个事件的一个订阅行为
     * @param {String} type 事件类型
     * @param {Function} cb 回调函数
     */
    unsubscribe(type, cb) {
        if (this.events[type]) {
            const targetIndex = this.events[type].findIndex(item => item === cb)
            if (targetIndex !== -1) {
                this.events[type].splice(targetIndex, 1)
            }
            if (this.events[type].length === 0) {
                delete this.events[type]
            }
        }
    }
    /**
     * 移除某个事件的所有订阅行为
     * @param {String} type 事件类型
     */
    unsubscribeAll(type) {
        if (this.events[type]) {
            delete this.events[type]
        }
    }
}
```
