# delta
The purpose of this project is to use margins of victory, or score differentials, to estimate the strength of a sports team. The central idea in the method I am presenting is that the total points a team scores minus the total points scored against them, corrected by a factor that accounts for the strength of teams played, can be used as a measure of overall team performance. The method or algorithm for computing the adjusted score differential, or ASD, consists of the following steps. 

First a note on notation. Symbols followed by one or more indexes should be read as "symbol, subscripted indexes" so **Gi** is **G-sub-i** and **&#948;j,k** is **&#948;-sub-i,j** where "**i, j**" is the subscripted two-dimensional index.

1. Construct an **i &#215; i** matrix, **S**, where i is the number of teams in the league being evaluated (NBA, NFL, MLB, etc.) and the entry **i, j** is equal to the number of points scored by team **i** in the match against team **j** minus the number of points scored in the match by **j** (unique integer indexes **[1, i]** being arbitrarily assigned to each team). Thus entry **i, j** in **S** is equal to the score differential of team **i** against **j** which will be positive if **i** won the match and negative if **i** lost. By definition the value of entry **i**, **i** is null or NaN.
2. Find the "zero-order" adjusted score differential for each team by computing the sum of score differentials for the team divided by the number of games that team played (ignoring any null values). Zero-order meaning no adjustments have been made.
3. For each team **i** construct the set of sets **Gi = {g1, g2, ..., gn}** where **gj** is a set of tuples representing each game played by team **j**, each tuple being of the form **(j, k, &#948;j,k)**, where **k** is the index of each opponent of **j** and **&#948;j,k** is the number of points scored by **j** minus the number of points scored by **k** in the match between **j** and **k**.
4. Remove from each  **gj**, where **j != i** in **Gi** the tuple that corresponds to the game between **i** and **j**, i.e., each match is represented twice in **Gi**, and we want to keep only one of each of the matches played by **i**.
5. To compute the first-order ASD of **i**, we first compute the adjustment factors for each **j != i** with respect to **i**. This factor **&#955;j,i** is equivalent to the zero-order ASD without including the match involving **i**. In other words, it is the sum of score differentials in **gj** divided by **|gj|** (the cardinality of **gj**).
6. We then take the formula for the zero-order ASD of **i**, but we add **&#955;j,i** to each **&#948;i,j** in the summation. This gives us the first-order ASD of **i**.
7. Even higher order ASDs can be calculated by adjusting each **&#955;j,i** in the same way we adjusted **&#948;i,j**, but because we eliminate matches each time, this cannot go on indefinitely for a league with a finite number of games.

Though this is all confusing in the abstract, I hope to develop and explain more of the subtleties of this measure, as well as provide an algoritm for automated computation of  various orders of ASD.
