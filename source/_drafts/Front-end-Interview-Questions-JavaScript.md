---
title: Front-end Interview Questions -- JavaScript
tags:
---
## 1. fetching data from an API

AJAX基础版
```
function fetchAPIByAJAX(method, url, callback, data=null) {
    const xhr = new XMLHttpRequest()
    // 第三个参数true表示异步执行
    xhr.open(method, url, true)
    xhr.onreadystatechange = function() {
        if(xhr.readyState === XMLHttpRequest.DONE) {
            if(xhr.status === 200) {
                callback(null, xhr.responseText)
            } else {
                callback(xhr.status)
            }
        }
    }
    if(method === "POST" && data) {
        xhr.setRequestHeader('Content-Type', 'application/json')
        xhr.send(JSON.stringify(data))
    } else {
        xhr.send()
    }
}
```
fetch基础版
```
function fetchAPI({url, method="GET", data=null, header={}}) {
    const defaultHeader = {
        'Content-Type': 'application/json'
    }
    const config = {
        url,
        method,
        headers: {...defaultHeader, ...header}
    }
    const queryParams = new URLSearchParams(data).toString()
    if(method === 'GET' && data) {
        url = `${url}?${queryParams}`
    } else if (data) {
        config.body = JSON.stringify(data)
    }
    return fetch(config).then(response => {
        if(!response.ok) {
            throw Error(response.status)
        }
        return response.json()
    }).catch(e => {
        throw Error(e)
    })
}
```
fetch增加超时取消功能
```
function fetchWithTimeout(config, timeout=3000) {
    return new Promise((resolve, reject) => {
        let timer = setTimeout(() => {
            reject(new Error('Time out'))
        }, timeout)
        fetchAPI(config).then(res => {
            clearTimeout(timer)
            resolve(res)
        }).catch(e => {
            clearTimeout(timer)
            reject(e)
        })
    })
}

function fetchRetry(config, timeout=3000, retry=3) {
    return fetchWithTimeout(config, timeout).catch(e => {
        if (retry > 0) {
            return fetchRetry(config, timeout, retry-1)
        } else {
            throw new Error(e)
        }
    })
}
```