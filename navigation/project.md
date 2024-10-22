---
layout: page
title: Project
search_exclude: true
permalink: /project/
---

import random

inventory = []
rooms_passed = 0
wrong_answers = 0
max_wrong = 3

questions = [
    {"text": "Do you want to open the chest? (yes/no)", "answer": "yes"},
    {"text": "Do you go through the left door or right door? (left/right)", "answer": "left"},
    {"text": "The riddle asks: I speak without a mouth and hear without ears. Who am I? (echo/silence)", "answer": "echo"},
    {"text": "Do you accept the puzzle challenge? (yes/no)", "answer": "yes"},
    {"text": "You see a sword on the ground. Pick it up? (yes/no)", "answer": "yes"},
    {"text": "Solve: What is 12 * 5? (enter a number)", "answer": "60"},
    {"text": "You encounter a locked door. Do you have a key? (yes/no)", "answer": "yes"},
    {"text": "Choose a door: 1, 2, or 3?", "answer": "2"},
    {"text": "There's a dark cave ahead. Enter? (yes/no)", "answer": "yes"},
    {"text": "Do you eat the mysterious food on the table? (yes/no)", "answer": "no"},
    {"text": "Solve: What is 25 + 37? (enter a number)", "answer": "62"},
    {"text": "You see a potion. Drink it? (yes/no)", "answer": "no"},
    {"text": "Do you challenge the monster? (yes/no)", "answer": "yes"},
    {"text": "The riddle asks: What has keys but can't open locks? (piano/lock)", "answer": "piano"},
    {"text": "You find a key on the floor. Pick it up? (yes/no)", "answer": "yes"},
    {"text": "There's a bridge ahead. Cross it? (yes/no)", "answer": "yes"}
]

def enter_room():
    global rooms_passed, wrong_answers
    print(f"\nYou enter a new room (Room {rooms_passed + 1}).")
    question = random.choice(questions)
    answer = input(question["text"] + " ").strip().lower()
    
    # Handle yes/no questions and puzzles
    if question["text"] == "Do you have a key? (yes/no)" and "key" in inventory:
        print("You use the key to unlock the door and proceed!")
    elif question["text"] == "You see a sword on the ground. Pick it up? (yes/no)" and answer == "yes":
        inventory.append("sword")
        print("You pick up the sword. It might come in handy!")
    elif question["text"] == "You find a key on the floor. Pick it up? (yes/no)" and answer == "yes":
        inventory.append("key")
        print("You now have a key in your inventory.")
    else:
        if answer == question["answer"]:
            print("Correct! You may proceed to the next room.")
            rooms_passed += 1
        else:
            print("Wrong answer.")
            wrong_answers += 1

    # Handle inventory and alternative paths
    if wrong_answers >= max_wrong:
        print("You made too many mistakes and are trapped in the room forever. Game over.")
        return False
    if rooms_passed >= 5:
        print("Congratulations! You passed through 5 rooms and won the game.")
        return False
    return True

def start_game():
    print("Welcome to the text-based adventure game! Can you navigate through 5 rooms and survive?")
    while rooms_passed < 5 and wrong_answers < max_wrong:
        if not enter_room():
            break

    if wrong_answers < max_wrong and rooms_passed >= 5:
        print("Victory is yours!")
    elif wrong_answers >= max_wrong:
        print("You lose... better luck next time.")

start_game()