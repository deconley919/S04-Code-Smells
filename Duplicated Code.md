# 2. Duplicated Code (Andy Zheng)

## A. Introduction

### What is the smell?

**Duplicated Code** often occurs when the same or very similar code appears in multiple locations within the same program or the same file. 

### How does it arise?

This often happens when developers copy and paste code or use two functions that are very similar to each other without the use of helper functions. 

This can also happens if multiple developers are working on the same project at the same time, but with poor communication where it causes two people to create the exact same function to help with their own code.

### Why is it a Problem?

Readability. If your code is long due to having multiple of the same functions without the use of helper functions or deleting some as they serve the same purpose, the code will be very hard to debug and solve. Additionally, if you find an error within the code, you would have to fix every instance of the problem across multiple functions.

## B. Prevention and Refactoring

### Preventing Duplicated Code

The easiest way to prevent is to first, **Identify** current functions, and this is to see if your current functions already support what you are trying to add.

### Fixing the problem

Look back through the code and identify the parts of the code where it could be refactored to improve readability and maybe condense multiple functions into one helper function. 

One of the methods you would want to use in refactoring would be to extract function, where you take out a block of code and replace it with a call to a function with that code. Usually this is used when you have multiple functions needing the same thing like calculating damage dealt.

## C. Code Examples

### JavaScript Example

#### Before
```
        function playerAttack(attack, defense) {
          let damage = attack - defense;
          if (damage < 0) damage = 0;
          return damage;
        }

        function enemyAttack(attack, defense) {
          let damage = attack - defense;
          if (damage < 0) damage = 0;
          return damage;
        }
```

#### What's wrong?

- **Two Functions, Exact Same Code:** Both functions, as you can easily see has the exact same code where it calculates damage where it takes defense from attack. This could go wrong easily as when you have say 6 enemies for attack calculations and you just discovered that you messed up in one part of the code. Because the code is the exact same, you now need to fix it every time.

#### After
```
        function calculateAttack(attack, defense) {
          let damage = attack - defense;
          if (damage < 0) damage = 0;
          return damage;
        }
```


### Python Example

#### Before
```
        def register_user(username):
            if len(username) < 3:
              print("Username too short")
              return
            print("User registered)

        def update_username(username):
            if len(username) < 3:
              print("Username too short")
              return
            print("Username updated)
```

#### What's wrong
- **Same Code in Functions, Two Functionalities** Both functions uses the exact same code, but one uses it to register for a user, while the other updates their username to the new username.

#### After
```
        def validate_username(username):
          if len(username) < 3:
              print("Username too short")
              return False
          return True

        def register_user(username):
            if(validate_username()):
                print("User registered)

        def update_username(username):
            if(validate_username()):
                print("Username updated)
```
