---
layout: page
title: 3-10-4
permalink: /Sprint2/
comments: true
---

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


##### Hacks List with user input
##### Using objects with different types and attributes
This code gets a persons name, age, and if they are a student or not with the users input. 

<span style="color: #4A7C2E; font-size: 22px;"> Using objects with different types and attributes</span>
