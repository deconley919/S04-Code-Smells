# S04-Code-Smells

Code Smells:
1. Mysterious Name(Kenneth Tran)

Introduction:
Mysterious Name is simply when a variable, function, class, or module has a name that fails to reveal its purpose or intent. By name a variable as such forces the reader or even the devoloper
to have to search through lines of code just to figure out what that thing does.

Prevention:
The easiest way to prevent this sort of thing is to simply just name your variable, function, class, or module what it does. For example instead of calling it x, it could be called totalCookies.

Refactoring:
The best way to refactor is to simply just replace the mysterious names with descriptive identifiers that relates to said part of the code.

Examples:
------------------------------------------------------------
    python:
        def up(c, n):
            a = n * 1.15
            if c >= a:
            return c - a
        return c
    
    Refactored:
        def purchase_cookie_upgrade(current_cookies, base_upgrade_cost):

            scaled_upgrade_cost = base_upgrade_cost * 1.15
            
            if current_cookies >= scaled_upgrade_cost:
            return current_cookies - scaled_upgrade_cost

        return current_cookies
------------------------------------------------------------
    C++:
        int c(int d){
            int v = d * 24;
            return v;
        }

    refactored:
        int convertDaysToHours(int days){
            int totalHours = days * 24;
            return totalHours;
        }
------------------------------------------------------------

2. Duplicated Code (Andy Zheng)

# 3. Large Class
Devin Conley
## A. Introduction

### What is the smell?

A **Large Class** is a bloater class which tries to do too much and signal poor distribution of responsibilites which makes code harder to read. 

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

- **Hard to understand** -> once you have a large class, it is easy to forget it's parts and you need to take time to read through the long section of code each time
- **Hard to safely edit** -> things can be tangled with unrelated features sharing fields, so changing one can break another
- **Duplication** -> logic gets trapped inside the large class, so code that needs it elsewhere tends to duplicate it 
- **Difficult to test** -> if a class is large enough, it becomes difficult to isolate and test things such as scoring rules 

## B. Prevention and Refactoring

### Preventing a Large Class

| Practice | How it helps |
|---|---|
| **Limit responsibilities**| Ask whether a method or field belongs in a class to do a specific and focused job. Helps prevent overly bloating classes as you expand your code |
|**Watch field prefixes**| Frequent use of prefixes such as `enemyX`, `enemySpeed`, `enemyHP` indicate a class avaiable to extract |
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
  score = new ScoreKeeper();
  audio = new AudioPlayer();

  damagePlayer(amount: number) {
    this.playerHealth.damage(amount);
    this.audio.sfx("hurt");
    if (this.playerHealth.isDead) this.gameOver();
  }
}
```