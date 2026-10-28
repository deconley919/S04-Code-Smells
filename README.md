# S04-Code-Smells

Code Smells:
# 1. Mysterious Name (Kenneth Tran)

## A. Introduction:

### What is the smell?

**Mysterious Name** is simply when a variable, function, class, or module has a name that fails to reveal its purpose or intent. 
By name a variable as such forces the reader or even the devoloper to have to search through lines of code just to figure out 
what that thing does.

### How does it arise?
The most common reason why:
**I'm too lazy** or **I'll do it later** -> Putting naming the thing off for later commonly becomes forgotten. This can scale up very fast
to the point where a developer could be asking themselfs "what did this do again?" when trying to revisit or change their old code.

## B. Prevention and Refactoring

### Preventing a Mysterious Name
The easiest way to prevent this sort of thing is to simply just name your variable, function, class, or module what it does. For 
example instead of calling it x, it could be called totalCookies.

### Fixing the problem
The best way to refactor is to simply just replace the mysterious names with descriptive identifiers that relates to said part of the code.

## C. Code Examples

### Python Example

#### Before
```
        def up(c, n):
            a = n * 1.15
            if c >= a:
            return c - a
        return c
```
#### What's wrong
- **What does up, c, n, a mean?:** Simple naming variables like this can become confusing very fast especially when a devloper needs to back track or revisit old code.

#### After
```     
        def purchase_cookie_upgrade(current_cookies, base_upgrade_cost):

            scaled_upgrade_cost = base_upgrade_cost * 1.15
            
            if current_cookies >= scaled_upgrade_cost:
            return current_cookies - scaled_upgrade_cost

        return current_cookies
```

### C++ Example

#### Before
```
        int c(int d){
            int v = d * 24;
            return v;
        }
``` 
#### What's wrong
- **What does c, d, v do mean?:** Once again we don't know what this section of code does. For all we know it could be completely unrelated to the program.

#### After
``` 
        int convertDaysToHours(int days){
            int totalHours = days * 24;
            return totalHours;
        }
```

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

# 3. Large Class (Devin Conley)

## A. Introduction

### What is the smell?

A **Large Class** is a bloater class which tries to do too much and signals poor distribution of responsibilities which makes code harder to read. 

It usually has:

- **Too many fields** -> "when a class has too many fields, duplicated code cannot be far behind" (Fowler pg. 82)
- **Too many methods** -> often don't talk to each other


### How does it arise?

It grows over time:

- **"Just one more feature"** -> it's always easier to add a method to an existing class than it is to create a new one.
- **Convenience** -> once a class stores `this.player` or `this.score` for example, every new feature wants access so they are added there.
- **Deadlines** -> sometimes a deadline is coming up and trying to split up a class might take more time 

### Why is it a problem?

A large class becomes a problem when it is difficult to make required changes.

- **Hard to understand** -> once you have a large class, it is easy to forget its parts and you need to take time to read through the long section of code each time
- **Hard to safely edit** -> things can be tangled with unrelated features sharing fields, so changing one can break another
- **Duplication** -> logic gets trapped inside the large class, so code that needs it elsewhere tends to duplicate it 
- **Difficult to test** -> if a class is large enough, it becomes difficult to isolate and test things such as scoring rules 

## B. Prevention and Refactoring

### Preventing a Large Class

| Practice | How it helps |
|---|---|
| **Limit responsibilities**| Ask whether a method or field belongs in a class to do a specific and focused job. Helps prevent overly bloating classes as you expand your code |
|**Watch field prefixes**| Frequent use of prefixes such as `enemyX`, `enemySpeed`, `enemyHP` indicate a class available to extract |
|**Small and frequent refactors**| Taking the time to review and refactor in parts along the way keep your code clean and readable while also avoiding a massive headache later |

### Fixing the problem

| Approach | When to use |
|---|---|
| **Extract Class** | If a subset of fields and methods are part of a similar concept (everything about the player), then move them into a new class |
| **Extract Superclass** | If two classes share a behavior, then move the shared part into a common parent the others can inherit |
| **Replace Type Code with Subclasses** | If you have a coded type that affects program behavior, then create a subclass for each type |
| **Extract Function** | If you have large methods, try to look to extract duplicated or unrelated parts and break things down |

## C. Code Examples

#### Before

```
class GameManager {
  playerHp = 100;
  playerMaxHp = 100;
  playerX = 0;
  playerY = 0;
  score = 0;
  highScore = Number(localStorage.getItem("highScore") ?? 0);
  musicVolume = 0.8;
  sfxVolume = 1.0;

  damagePlayer(amount: number) {
    this.playerHp = Math.max(0, this.playerHp - amount);
    this.playSfx("hurt");
    if (this.playerHp === 0) this.gameOver();
  }

  movePlayer(dx: number, dy: number) {
    this.playerX += dx;
    this.playerY += dy;
  }

  addScore(points: number) {
    this.score += points;
    if (this.score > this.highScore) {
      this.highScore = this.score;
      localStorage.setItem("highScore", String(this.highScore));
    }
  }

  playSfx(name: string) {
    new Audio(`sfx/${name}.wav`).volume = this.sfxVolume;
  }

  gameOver() {
  }
}
```

#### What's wrong

- **Several responsibilities in one class:** player health and movement, score tracking, music and sfx (notice matching prefixes/suffixes)
- **Too many reasons to change:** a tweak to health rules, score, or audio all mean editing this same class
- **Nothing can be reused:** once you want to add an enemy which also needs HP, you would have to make a copy of `damagePlayer()` somewhere in the code

#### After

```
class Health {
  constructor(public hp = 100, public max = 100) {}
  damage(amount: number) { this.hp = Math.max(0, this.hp - amount); }
  get isDead() { return this.hp === 0; }
}

 class Position {
  constructor(public x = 0, public y = 0) {}
  move(dx: number, dy: number) { this.x += dx; this.y += dy; }
}


class Enemy {
  health = new Health(50, 50);
  constructor(public name: string) {}
}

class ScoreKeeper {
  score = 0;
  highScore = Number(localStorage.getItem("highScore") ?? 0);
  add(points: number) {
    this.score += points;
    if (this.score > this.highScore) {
      this.highScore = this.score;
      localStorage.setItem("highScore", String(this.highScore));
    }
  }
}

class AudioPlayer {
  constructor(public sfxVolume = 1.0, public musicVolume = 0.8) {}
  sfx(name: string) { new Audio(`sfx/${name}.wav`).volume = this.sfxVolume; }
}

class GameManager {
  playerHealth = new Health();
  playerPos = new Position();
  score = new ScoreKeeper();
  audio = new AudioPlayer();
  enemies: Enemy[] = [new Enemy("slime"), new Enemy("bat")];

  damagePlayer(amount: number) {
    this.playerHealth.damage(amount);
    this.audio.sfx("hurt");
    if (this.playerHealth.isDead) this.gameOver();
  }

  damageEnemy(enemy: Enemy, amount: number) {
    enemy.health.damage(amount);
    this.audio.sfx("hit");
    if (enemy.health.isDead) {
      this.score.add(10);
      this.enemies = this.enemies.filter(e => e !== enemy);
    }
  }
}

gameOver() {
}
```