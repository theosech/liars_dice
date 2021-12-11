# liars_dice

# Bayesian Inference problem
Infer the highest utility action from the following (Actions):
- Call previous guess
- New guess

Each possible outcome corresponds to the following rewards (Rewards):
* (1 / (num_players-1)) if I succesfully call someone
* -1 if I unsuccessfully call someone
* -1 if my guess gets successfully get called
* (1 / (num_players-1)) if my guess get unsuccessfully called
* 0 if my guess does not get called
    
### Pior Probability Model:
    We have a prior over distribution of total dice. When selecting a bet
    we use that to calculate the probability of each bet being true and the rewards
    for each of the cases to select the move that maximizes the expected value

    At each bet we update:
      - Nothing

### Other players are dumber than me Model:
    Same as before but now every time someone guesses, we update our belief about
    the dice they have assuming they select their best action using what (I think) they have
    and a prior about other players (the same for all players; we implicitly assume their 
    strategy is simpler than ours)

    At each bet we update: 
      - My beliefs about the bettor

### Other players think like me Model:
    Same as before but now every time someone guesses, we update our belief about
    the dice they have assuming they select their best action using what (I think) they have
    and the same beliefs I have about other players

    At each bet we update: 
      - My beliefs about the bettor

### TOM Model:
    We assume every other player has their own unique beliefs about other players (and me)
    and update these for every player for every bet

    At each bet we update: 
      - My beliefs about the bettor
      - Every other player's beliefs about the bettor

### TOM longer time horizon (could model bluffing):
    Bluffing can be modeled as optimal when the expected reward is large as a result of 
    distorting others' model of our dice

Optimal Bayesian Inference:
Using Bayes rule, we can combine the prior probability of a bet being true, with the
likelihood of that bet being proposed given the proposer's information where we assume
they would select the highest expected utility move for them using information about
their dice. To calculate this likelihood we are back to the same problem of selecting
the MAP action given one's dice. This would continue until we got to the first player
who would only use prior information to select the best action.

How to approximate efficiently:
We start with a prior belief on the dice of each player. Every time someone bets, we update
the prior belief about their dice assuming that when doing so, they used the same inferred
distributions of other players as we have.

How to compute more accurately:
We start with a prior belief on the dice of each player. Every time someone bets, we update
the prior belief about their dice. We also compute and store the beliefs of each player
for the rest where we use our best estimate of their dice. Thus for each bet, we update
the model of the bettor for each of the players in the game (with the only different that 
the distribution of our dice is deterministic)


We want to write a function that given:
    - The distribution over dice I have for the bettor
    - The distribution over dice for each player from the perspective of the player that proposed the bet
    - The proposed bet
Returns:
    - The updated distribution over dice I have for the bettor
    - The updated TOM distribution over dice I belive other people have for the bettor
    
We want to write a function that given:
    - The distributions for all players (exact dice for myself)
    - The proposed bet
Returns:
    - The probability of the bet being true
    
We want to write a function that given:
    - The distribution for all players (exact dice for myself)
    - The probability of the bet being true
    - The possible next moves
    - The rewards for each move including calling
Returns:
    - The highest expected reward move
