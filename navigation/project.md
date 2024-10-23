---
layout: page
title: Project
permalink: /oroject/
---

import random

<script>
deck = list(range(1, 14)) * 4  # Four suits, values 1-13
random.shuffle(deck)

def card_name(value):
    """Helper function to return the name of the card."""
    names = {1: "Ace", 11: "Jack", 12: "Queen", 13: "King"}
    return names.get(value, str(value))

def higher_or_lower():
    score = 0
    current_card = deck.pop()  # Draw the first card
    print(f"Starting card is: {card_name(current_card)}")

    while len(deck) > 0:
        next_card = deck.pop()  # Draw the next card
        guess = input("Will the next card be higher or lower? (h/l): ").lower()
        
        if guess not in ["h", "l"]:
            print("Invalid input! Please enter 'h' for higher or 'l' for lower.")
            continue

        print(f"Next card is: {card_name(next_card)}")

        # Compare the cards
        if guess == "h" and next_card > current_card:
            print("Correct! It was higher.")
            score += 1
        elif guess == "l" and next_card < current_card:
            print("Correct! It was lower.")
            score += 1
        else:
            print("Wrong guess!")

        current_card = next_card
        print(f"Your score is now: {score}")
        play_again = input("Do you want to keep playing? (y/n): ").lower()
        if play_again != 'y':
            break

    print(f"Game over! Your final score is: {score}")

# Run the game
higher_or_lower()
</script>