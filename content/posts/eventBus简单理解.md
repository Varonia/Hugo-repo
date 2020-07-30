---
title: "EventBus简单理解"
date: 2020-07-30T23:12:20+08:00
draft: true
---

什么是EventBus？其实就是事件的订阅与发布，广泛用于页面之间的传值，原理：
![原理图](https://resource.liujiaweb.com/wp-content/uploads/2018/11/eventBusE58E9FE79086-768x288.png)

##实现EventBus
`
// 构造EventBus
function EventBusClass() {
    this.msgQueues = {}
}

EventBusClass.prototype = {
    // 将消息保存到当前的消息队列中
    on: function(msgName, func) {
        if (this.msgQueues.hasOwnProperty(msgName)) {
            if (typeof this.msgQueues[msgName] === 'function') {
                this.msgQueues[msgName] = [this.msgQueues[msgName], func]                
            } else {
                this.msgQueues[msgName] = [...this.msgQueues[msgName], func]    
            }
        } else {
            this.msgQueues[msgName] = func;
        }
    },
    // 消息队列中仅保存一个消息
    one: function(msgName, func) {
        // 无需检查msgName是否存在
        this.msgQueues[msgName] = func;
    },
    // 发送消息
    emit: function(msgName, msg) {
        if (!this.msgQueues.hasOwnProperty(msgName)) {
            return
        }
        if (typeof this.msgQueues[msgName] === 'function') {
            this.msgQueues[msgName](msg)
        } else {
            this.msgQueues[msgName].map((fn) => {
                fn(msg)
            })
        }
    },
    // 移除消息
    off: function(msgName) {
        if (!this.msgQueues.hasOwnProperty(msgName)) {
            return
        }
        delete this.msgQueues[msgName]
    }
}

// 将EventBus放到window对象中
const EventBus = new EventBusClass()
window.EventBus = EventBus
`

##使用EventBus
`
// 订阅消息
function subscribe() {
    EventBus.on('first-event', function(msg) {
        alert(`订阅的消息是：${msg}`);
    });
}

// 发送消息
function emit() {
    const msgInput = document.getElementById("msgInputId")
    EventBus.emit('first-event', msgInput.value)
}

// 移除消息
function off(msgName) {
    EventBus.off(msgName)
}

`



