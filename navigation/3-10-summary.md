---
layout: page
title: 3-10 Summary 
permalink: /3-10-summary/
comments: true
---

3.10.4 List Input
Focus: Student-led teaching on lists
Objective: Learn about storage and manipulation of multiple items using indexing
Date: Sep 25, 2024
Author: Zach
Read Time: 1 min

CSP Big Ideas
Hacks List with User Input
Using objects with different types and attributes
Code Overview
Purpose: Collects user information (name, age, student status) and stores it in a list.
python
Copy code
# Define a list to hold user information
people = []

# Function to collect user input and create a person object
def add_person():
    name = input("What is your name? ")
    age = input("How old are you? ")
    
    # Simple Yes/No question for student status
    while True:
        is_student = input("Are you a student? (yes/no): ").lower()
        if is_student in ["yes", "no"]:
            is_student = (is_student == "yes")  # Converts to Boolean
            break
        else:
            print("Please enter 'yes' or 'no'.")

    # Create the person object
    person = {
        'name': name,
        'age': age,
        'is_student': is_student
    }
    
    # Add the person to the list
    people.append(person)

    # Display the added person
    display_people()

# Function to display the list of people
def display_people():
    print("\nCurrent List of People:")
    for index, person in enumerate(people, 1):
        student_status = "a student" if person['is_student'] else "not a student"
        print(f"Person {index}: {person['name']}, {person['age']} years old, {student_status}")

# Example Usage
add_person()  # Call this to start collecting input from the user
Example Output
Current List of People:
Person 1: Zach, 16 years old, a student
Homework Assignment
Create a List: Formulate a list with yes or no answers and two questions.
Convert the Code: Adapt the code for easier accessibility in Python.
