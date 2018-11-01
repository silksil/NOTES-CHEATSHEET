**Imperative Pattern**  focuses on describing how a program operates, it consists of commands for the computer to perform.

**Declarative Pattern**  focuses on what the program should accomplish without specifying how the program should achieve the result. Functional programming follows declarative pattern.

```javascript
let books = [
  {name:'JavaScript', pages:450}, 
  {name:'Angular', pages:902},
  {name:'Node', pages:732}
];

/* Imperative Pattern */
for (var i = 0; i < books.length; i++) {
  books[i].lastRead =  new Date();
}

/* Declarative Pattern */
books.map((book)=> {
  book.lastReadBy = 'me';
  return book;
});

```

Imperative code frequently utilizes statements. A statement is a piece of code which performs some action. Examples of commonly used statements include for, if, switch, throw, etc…

Declarative code relies more on expressions. An expression is a piece of code which evaluates to some value. Expressions are usually some combination of function calls, values, and operators which are evaluated to produce the resulting value.
