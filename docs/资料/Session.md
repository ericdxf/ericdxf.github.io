### Session手动配置

F12控制台输入下方代码

```
localStorage.setItem('SESSION','NDE2NDNmMzEtOTE4YS00ODZmLWI0MmQtMzcwZDkyYWZmYWQ3')
```



```
localStorage.setItem('name','Bob')
console.log(localStorage.getItem('name'))
localStorage.removeitem('Bob')
 
sessionStorage.setItem('name','Bob')
console.log(sessionStorage.getItem('name'))
sessionStorage.removeitem('Bob')
 
document.cookie='name = kyle'
 
document.cookie='name = kyle expires=' + new Date('2020', 0,1).toUTCString
```

