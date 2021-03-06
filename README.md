# Artificial-Intelligence-BattleGround-PlayerBot

The task for the playerBot and rules of the game:

It was a foggy and quiet night we hadn't seen in months.
Shipwrecks, planks and remnants of ship's equipment could be seen for miles away from the scene of the clash. After a battle that will be remembered as the biggest pirate skirmish in history, the turbulent sea has finally returned to balance.
Captain Vukobradi used to be the scariest pirate in the Stormy Sea. He had the largest and fastest ship, with the strongest, most ruthless crew. The treasure he stole after the fateful battle was invaluable.
The battle is over, but at what cost?
The fleet on both sides was blown to pieces.
In addition to the many lives lost, the Captain knew that he did not have much time left before the next battle. As soon as word of his weakened crew spread, other pirate captains would seize the opportunity. The desire not to lose his reputation, respect and his treasure pushed him into a corner. He had to act fast.
Pirate voyages were full of dangers, both from the open sea and from wild, mysterious creatures that need to be dealt with. After so many years on the high seas, the Captain knew the sea like a little finger. He knew exactly where and in what way to bury the treasure so that it would not be found, which he did.
Then ... several small waves lapped the Captain's ship. That was it!
When a storm strikes, every captain will become greedy and eager for destruction, which is an ideal time to attack.
Any rival ship can easily sneak up on you in these unfavorable conditions.
Help your crew find lost treasure and become the most notorious fleet on the high seas.

AI Battleground 2021 is a TBS (Turn Based Strategy) game in which four artificial intelligences take on the role and powers of an avatar to fight for victory. The moves take place in order, from the first to the fourth player.
You are on a sea of **hexagonal shape**, 29 diagonals long, and you can't get out of it. The map is of **constant size**. The game can last a maximum of **5000** moves, ie. 1250 moves per player.
Gold coins (points) can be won in several ways. At all times, there is a flag on the map. The player who reaches her first wins the flag, as well as the gold coins she hides. In addition, the player can win gold by eliminating opponents or NPCs (Non-Playable Characters).
The task is to create a computer program that will play the game as one of the pirate ships, as intelligently as possible, by sending requests to the server and obtaining server responses in JSON format.

Maps are generated depending on the round of the competition. The closer you get to the finals, the harder the map gets. The map is fair to all players.


![alt text](https://github.com/Una865/Artificial-Intelligence-BattleGround-Bot/blob/main/Map.png)

## Fields:
**1.Normal:**
here players can move. However, vortex-shaped obstacle can also be found in the field, as well as pirates.
**2.Island**
there is an island in this field. The islands are impassable. There may be a shop or a flag on the island.

The map is generated based on the JSON sent by the server. The following are examples of fields that can be found on the map. If the field is normal, a vortex or nothing can be found on it. In each field on the map you can find:
- boat (avatar or NPC)
- shop
- islandflag
- vortex

**Player number one starts the game with coordinates (-7, -7.14), player number two with coordinates (14, -7, -7), player number three with coordinates (7.7, -14) and player number four with coordinates (-14,7,7).
At the beginning of the game, two NPCs are also created on the map. They circle a fixed radius from the center of the map until they see one of the players, after which they start chasing them. If a player manages to escape them, they return to their course by the shortest route.**

## Coordinate system

The map uses an offset coordinate system based on offsets from the main diagonals. The three coordinates that are tracked are:
- q : marks the offset from the main diagonal;
- r : marks the offset from the midline;
- s : marks the offset from the side diagonal;
**An important feature of this coordinate system is always valid q + r + s = 0**

![alt text](https://github.com/Una865/Artificial-Intelligence-BattleGround-Bot/blob/main/CoordinateSystem.png)

Attributes of the avatar:
- **ID (id)** : avatar identification number;
- **Coordinates (q, r, s)** : indicate the position on the map (coordinate system is explained)
later in the text);
- **Gold coins** : which are collected by winning a flag, eliminating an opponent or
NPCs;
- **Illegal moves (illegalMoves)** : illegal moves include all illegal actions, ie.
actions that cannot be performed. For every 10 illegal moves, the avatar is reduced
life points for 5% of maximum life points;
- **Health points** : start from a maximum of 400. If health drops to 0,
your avatar is out of the game;
- **Maximum life points (maxHealth)** - represents the maximum number of lives
points that an avatar may have;
- **Cannons** : starts at 100, and represents the number of life points you have
the avatar takes off the opponent after the attack;
- **Paralyzed** - shows if your avatar is paralyzed. trapped in a whirlpool;
- **WhirlpoolDuration** : shows how many more moves your avatar has captured
in the vortex and cannot get out of it, after entering the vortex, this value is set to 3;
- **Number of potions (potNums)** : the number of potions your avatar has with them. Your avatar can
have a maximum of 3 drinks;
- **Sight** : represents how many fields the avatar can see. If
the opposing player is closer or at this distance, in the JSON response from the server you will be able to see the values of all attributes of his avatar, marked as playarX, where X is the ordinal number of the opposing player.

## Move

Each player must make an order for one action per move. Possible actions are:
Navigate the map for one field
- If the field on which the avatar wants to move is an island, moving is not possible;
- If the field is out of the folder, scrolling is not possible;
- If there is another player on the field, it is not possible to move;
- Actions for movement are: nw - up, left - left, sw - down, left - down, e -
right, not -right. Shopping from the store
- Purchases from the store are only possible from the surrounding store areas.
- The purchase avatar must have enough gold coins for the transaction it wants
to do.
- Possible transactions:
- If an avatar wants to buy a medicinal drink, he can only do so if he currently has a maximum of two drinks. The required number of gold coins is 150, while the action is: "buy-POTION";
- An avatar can improve their health twice during the game. The price of the first promotion is 200, while the second is 300, and the action is: "buy-HULL";
- An avatar, during the game, can only double the amount of life points that his attack takes away from the opponent. The price of the first promotion is 200, while the second is 300, and the action is: "buy-CANNONS".
- Improvements:
- Cannons: 100-> 150-> 250;
- Life points: 400-> 600-> 1000; Use of medicinal drink
- In order to use a medicinal drink, an avatar must have at least one medicinal drink;
- Medicinal drink increases life points by 10% of maximum life points, while the action is "up";
Attacking another avatar or NPC
- You can only attack an opponent if it is within range, ie. if it is at most 2 fields away from you;
- Attack reduces the opponent's life points by the strength of your cannons (value cannons variable), while the action is "atk-X", where X is the ordinal number of your opponent (1, 2, 3 or 4);
- Elimination of the opponent gives the avatar 250 gold coins.

## Going for a flag

The flag is won by the player who first comes in the immediate vicinity of the island where the flag is located (when the distance between the avatar and the flag is equal to 1). Winning the avatar flag brings 500 gold coins;
- It is not necessary to issue a special command to win the flag.

## Wining

Any avatar that loses all life points is eliminated from the game.
The game ends after the expiration of 5000 moves or after the elimination of all but one avatar.
If the game ends with the elimination of 3 avatars, the winner is the avatar that remained in the game. Ranking
the remaining three avatars are performed in the order of elimination from the game (the first is eliminated last, the second is eliminated penultimate, etc.).
If the game ends after the expiration of 5000 moves, the winner is the one who collected the most gold coins. If several avatars have the same number of gold coins, they are ranked according to the number of remaining life points. If several avatars have the same number of gold coins and the same number of life points, then you can see how many life points their attack removes (cannons attribute).




 


 
