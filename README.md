# Thierry-I-224006696
Here’s how to implement this Hangman game step by step in Python:

Step 1: Load a list of words from a file

1. Create a words.txt file with a list of words (one per line).


2. Write a function to randomly select a word from the file.



import random

def load_words(filename):
    with open(filename, 'r') as file:
        words = file.read().splitlines()
    return words

def choose_word(word_list):
    return random.choice(word_list)


---

Step 2: Set up game initialization

Create variables for guesses, warnings, the secret word, and guessed letters.

Display the initial game state.


def initialize_game(word_list):
    secret_word = choose_word(word_list)
    guesses_remaining = 10
    warnings_remaining = 3
    guessed_letters = set()
    print("Welcome to Hangman!")
    print(f"The word has {len(secret_word)} letters.")
    print("-" * 20)
    return secret_word, guesses_remaining, warnings_remaining, guessed_letters


---

Step 3: Update the game state after each guess

Display the current state of the word.

Track guessed letters and provide feedback.


def display_word(secret_word, guessed_letters):
    return ''.join([char if char in guessed_letters else '-' for char in secret_word])

def update_guesses(guess, secret_word, guesses_remaining):
    vowels = "aeiou"
    if guess in secret_word:
        return guesses_remaining
    elif guess in vowels:
        return guesses_remaining - 2
    else:
        return guesses_remaining - 1


---

Step 4: Handle invalid input and warnings

Check for repeated or invalid inputs.

Reduce warnings or guesses accordingly.


def handle_invalid_input(guess, guessed_letters, warnings_remaining, guesses_remaining):
    if guess in guessed_letters:
        if warnings_remaining > 0:
            warnings_remaining -= 1
            print("You've already guessed that letter. Warning lost.")
        else:
            guesses_remaining -= 1
            print("You've already guessed that letter. Guess lost.")
    elif not guess.isalpha():
        if warnings_remaining > 0:
            warnings_remaining -= 1
            print("Invalid input. Warning lost.")
        else:
            guesses_remaining -= 1
            print("Invalid input. Guess lost.")
    return warnings_remaining, guesses_remaining


---

Step 5: Check for win/loss conditions

Determine if the user has won or lost.


def check_win(secret_word, guessed_letters):
    return all(char in guessed_letters for char in secret_word)

def calculate_score(guesses_remaining, secret_word):
    unique_letters = len(set(secret_word))
    return guesses_remaining * unique_letters


---

Step 6: Main game loop

Combine everything into a loop.


def hangman():
    word_list = load_words("words.txt")
    secret_word, guesses_remaining, warnings_remaining, guessed_letters = initialize_game(word_list)
    
    while guesses_remaining > 0:
        print(f"Guesses remaining: {guesses_remaining}")
        print(f"Warnings remaining: {warnings_remaining}")
        print(f"Available letters: {''.join(sorted(set('abcdefghijklmnopqrstuvwxyz') - guessed_letters))}")
        print("Current word:", display_word(secret_word, guessed_letters))
        
        guess = input("Enter your guess: ").lower()
        
        if guess.isalpha() and len(guess) == 1:
            if guess in guessed_letters:
                warnings_remaining, guesses_remaining = handle_invalid_input(guess, guessed_letters, warnings_remaining, guesses_remaining)
            else:
                guessed_letters.add(guess)
                guesses_remaining = update_guesses(guess, secret_word, guesses_remaining)
        else:
            warnings_remaining, guesses_remaining = handle_invalid_input(guess, guessed_letters, warnings_remaining, guesses

