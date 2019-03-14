## 10 Javascript Performance Boosting Tips from Nicholas Zakas

1. Define local variables(定义变量)
   When a variable is referenced, Javascript hunts it down by looping through the different members of the scope chain. This scope chain is the set of variables available within the current scope, and across all major browsers it has at least two items: a set of local variables and a set of global variables.
   Simply put, the deeper the engine has to dig into this scope chain, the longer the operation will take. It first goes through the local variables starting with this, the function arguments, then any locally defined variables, and afterwards iterates through any global variables.
   Since local variables are first in this chain, they’re always faster than globals. So anytime you use a global variable more than once you should redefine it locally, for instance change:
   ```js
   var blah = document.getElementById('myID'),
   blah2 = document.getElementById('myID2');
   ```
   to
   ```js
   ```

2. Don't use the `with()` statement(不要使用 `with()`)
   It’s pretty much a fact: the with() statement is pure Javascript evil.
   This is because with() appends an extra set of variables to the beginning of the scope chain described above. This extra item means that anytime any variable is called, the Javascript engine must loop through the with() variables, then the local variables, then the global variables.
   So with() essentially gives local variables all the performance drawbacks of global ones, and in turn derails Javascript optimization.
   
3. Use closures sparingly(谨慎使用闭包)

4. Object properties and array items are slower than variables(对象属性和数组项比变量慢)

5. Don't dig too deep into arrays(不要深层次挖掘数组)

6. Avoid `for-in` loops(and function based iteration)(避免使用 `for-in`)

7. Combine control conditions and control variable changes when using loops(使用循环时结合控制条件和控制变量的改变)

8. Define arrays for HTML collection objects(为HTML集合对象定义数组)

9. Stop touching the DOM(禁止操作 `DOM`)

10. Change CSS classes not styles(更改CSS类而不是样式)
