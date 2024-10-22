---
layout: page
title: Project
search_exclude: true
permalink: /project/
---
<script>
import random

# Define the ranks and values for the cards
ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
values = {'2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 10, 'J': 11, 'Q': 12, 'K': 13, 'A': 14}

# Create a deck of 52 cards
deck = ranks * 4
random.shuffle(deck)

# Split deck between two players
player1_deck = deck[:26]
player2_deck = deck[26:]

# Function to compare two cards
def compare_cards(card1, card2):
    if values[card1] > values[card2]:
        return 1  # Player 1 wins
    elif values[card1] < values[card2]:
        return 2  # Player 2 wins
    else:
        return 0  # Tie

# Main game loop
while player1_deck and player2_deck:
    # Both players draw their top card
    card1 = player1_deck.pop(0)
    card2 = player2_deck.pop(0)
    print(f"Player 1 draws {card1} and Player 2 draws {card2}")

    # Compare the cards
    result = compare_cards(card1, card2)

    if result == 1:
        print("Player 1 wins the round and takes both cards!")
        player1_deck.extend([card1, card2])
    elif result == 2:
        print("Player 2 wins the round and takes both cards!")
        player2_deck.extend([card1, card2])
    else:
        print("It's a tie! War!")
        # Handle a tie (War)
        war_pile = [card1, card2]
        
        if len(player1_deck) < 4 or len(player2_deck) < 4:
            print("One player doesn't have enough cards for war! The other player wins!")
            break
        
        for _ in range(3):  # Each player places 3 cards down (face-down)
            war_pile.append(player1_deck.pop(0))
            war_pile.append(player2_deck.pop(0))
        
        # New cards drawn to resolve war
        card1 = player1_deck.pop(0)
        card2 = player2_deck.pop(0)
        print(f"Player 1 draws {card1} and Player 2 draws {card2} for war!")

        result = compare_cards(card1, card2)
        if result == 1:
            print("Player 1 wins the war and takes the pile!")
            player1_deck.extend(war_pile + [card1, card2])
        elif result == 2:
            print("Player 2 wins the war and takes the pile!")
            player2_deck.extend(war_pile + [card1, card2])
        else:
            print("Another tie during war! Both players leave the cards.")

# Determine winner
if player1_deck:
    print("Player 1 wins the game!")
else:
    print("Player 2 wins the game!")
</script>