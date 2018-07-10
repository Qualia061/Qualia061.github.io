# Project Euler 84

## Description
The game Monopoly is played on a board with 40 squares, set up as shown below.

![main](https://github.com/Qualia061/Data-Science-Projects/blob/master/Project%20Euler%2084/pics/main.png)

Players begin the game on the GO square. On each turn, the player rolls two 6-sided dice. The sum of the dice determines the number of squares they advance (in a clockwise direction) on that turn.


Without any further rules, we would expect players to visit each square with equal probability. However, landing on G2J (go to jail), CC (community chest), and CH (chance) changes this distribution. When a player lands on G2J, they must immediately move to the JAIL square. 


At the beginning of the game, the CC and CH cards are shuffled. When a player lands on CC or CH they take a card from the top of the respective pile and, after following the instructions, it is returned to the bottom of the pile. There are 16 cards in each pile, but for this problem we are only concerned with cards that order a movement. Any card not concerned with movement will be ignored and the player will remain on the CC/CH square. The relevant cards are:

• Community Chest (2/16)

1. Advance to GO

2. Go to JAIL

• Chance (10/16)

1. Advance to GO

2. Go to JAIL

3. Go to C1
 
4. Go to E3
 
5. Go to H2

6. Go to R1
 
7. Go to next RR (railroad)
 
8. Go to next RR

9. Go to next UT (utility)

10. Go back 3 squares


In addition to G2J and CC/CH, if a player rolls doubles on three consecutive turns, they proceed directly to jail. A “double” is a roll where the two dice are equal, such as rolling 2 fives or 2 sixes.

The goal is to estimate the long-term probabilities of landing on each square. That is, to compute for each square the probabilty that the player will end a turn that square if they play the game for infinitely many turns.


## Analysis

The word “long-term” means when the n goes to infinity. Since I have to choose a finite value of n, I set n=100000 and the position of the square GO is 0. The three most likely squares to end a turn with 6 sided dice are Jail (Position 10, 5.86%), E3(Position 24, 3.20%) and GO (Position 0, 3.17%). The three most likely squares to end a turn with 4 sided dice are Jail (Position 10, 6.41%), RR2 (Position 15, 3.53%), E3(Position 24, 3.29%).

![prob](https://github.com/Qualia061/Data-Science-Projects/blob/master/Project%20Euler%2084/pics/prob.png)

By comparing the plots, I found that the trends are almost identical for different number of sides. The GO square and the JAIL square are the most popular squares. Position 30 is the G2J square. Since the player has to immediately move to JAIL, so the probability of ending on G2J is 0.

The standard error for the long-term probability of ending a turn in jail with the 6-sided dice is about 6.6257E-5. (n=10000 turns, k=1000 simulations)
 
 
Also, I would like to try non-parametric bootstrap with b = 1,000 samples from a simulation of n = 10,000 turns.

I set b=1000 samples and n=10000 simulations for bootstrapping. For the 6-sided dice, the standard error using the bootstrap estimate is about 7.7203E-5, which is larger than that of the simulation estimate. The simulation estimate is more accurate since the standard error of the bootstrap method is larger than that of the simulation estimate. This makes sense since when using the bootstrap method, we only play one game and the result may be biased. For the simulation estimate, we simulate the game 1000 times and the distribution is closer to the population distribution.  

The time duration of the bootstrap method is about 11.41 seconds. The time duration of the simulation method is about 2.50 minutes. I noticed the bootstrap method is much faster. To compute the bootstrap estimate, we only run the simulation once (10000 turns) and reselect the samples, but the simulation estimate requires us to run the simulation 1000 times, resulting in 1000*100000 turns. Thus, the bootstrap estimate is much faster.


I used the simulation estimate to calculate the stand errors of the long-term probabilities for 3,4,5,and 6 sided dice, with n=10000 turns and k=1000 simulations. 

![se](https://github.com/Qualia061/Data-Science-Projects/blob/master/Project%20Euler%2084/pics/se.png)

From the plots above, we notice that the standard errors of the position 10, the Jail square, are always the largest one. This is reasonable since the player may pick the “GO to JAIL” cards multiple times in one game, but it is also possible that he does not pick the card and does not go to Jail even once. Therefore, the variation may be larger, which results in the large standard error.

As n increases, the standard errors for the long-term probability estimates decreases. The formula of the standard error is the standard deviation divided by the square root of the sample size. Thus, as the sample size increases, the sample means become more closely to the population mean and the standard error decreases. If the sample size goes to infinity, the standard error will be 0.

To verify my explanations, I calculated the standard errors of ending a turn on the square Jail with different n values and plotted a bar plot, as shown below.

![see](https://github.com/Qualia061/Data-Science-Projects/blob/master/Project%20Euler%2084/pics/see.png)

We can see that the standard error decreases as the n value increases, which is what I expected and it is true for all squares. 
