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
3. Large Class (Devin Conley)

There is a temporary markdown file for each smell so we can work without risking merge conflicts.