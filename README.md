<<<< [back to assessment resources](https://github.com/dr-matt-smith/FEDev---assessment-samples-and-walkthroughs)

---

[Stage 1](https://github.com/dr-matt-smith/FEDev-form-validation-with-user-error-messages--stage-1)
| 
Stage 2
|
[Stage 3](https://github.com/dr-matt-smith/FEDev-form-validation-with-user-error-messages--stage-3)
|
[Stage 4](https://github.com/dr-matt-smith/FEDev-form-validation-with-user-error-messages--stage-4)
|
[Stage 5](https://github.com/dr-matt-smith/FEDev-form-validation-with-user-error-messages--stage-5)



# FEDev-form-validation-with-user-error-messages


# stage 2 - throw error when should not create TODO

The project has 2 form validation issues:

1. an empty todo string might be submitted
   - while we could reduce the chance of an empty string by adding a `required` attribute to the form input, a bad actor could still send an empty string when bypassing HTML form attributes
2. duplicate TODOs can be created with identical text
   - let's assume we do not wish to have multiple, identical TODOs in our list

## do NOT create new TODO in problem situations

For the 2 cases above, we'll throw an `Error()` and not created a TODO when they're spotted

Let's edit the `cfreateTodo()` function in file `/lib/server/database.js` to look as follows:

`/lib/server/database.js`


```javascript
  ... other code here ...

  export function createTodo(userid, description) {
    // error 1 - empty description text
    if (description === '') {
      throw new Error('todo must have a description');
    }
  
    const todos = db.get(userid);
  
    // error 2 - text is identical to an existing TODO
    if (todos.find((todo) => todo.description === description)) {
      throw new Error('todos must be unique');
    }
  
    todos.push({
      id: crypto.randomUUID(),
      description,
      done: false
    });
  }
```

This works!
if either error condicatiopn occurs, our website will not create a new TODO, it will show us an error 500 page something like this:

![500 error page](/screenshots/20_error_page_500.png)

we'll improve the user experience in the next steps...
