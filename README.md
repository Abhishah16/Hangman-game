# Hangman-game
Playability
I ran your game to test it out first, and noticed something odd when a letter is found twice in the same word, such as "glass" here:

Pick a letter
 A
You guessed correctly!
['-', '-', 'a', '-', '-']
Pick a letter
 s
You guessed correctly!
['-', '-', 'a', 's', '-']
['-', '-', 'a', 's', 's']
I'm not sure if that is by design, but as a player I find it odd that it's displayed twice. I found where it originated in the code:

for x in range(0, length_word): #This Part I just don't get it
    if secretWord[x] == guess:
        guess_word[x] = guess
        print(guess_word)
If you just unindent print(guess_word) by two levels, you'll avoid that behavior since it won't run in a loop:

for i in range(0, length_word):
    if secretWord[i] == guess:
        guess_word[i] = guess
print(guess_word)
Printing a raw array like ['-', '-', 'a', 's', 's'] is a bit confusing, at first I didn't know what it was for until I guessed one right and saw the results. So let's make it print something more friendly:

print("Word to guess: {0}".format(" ".join(guess_word)))
Pick a letter
 o
You guessed correctly!!
Word to guess: - o -
Much better! But it will get a bit clunky to type all that each time, so let's make a utility function for it and we can use it for the print(guess_word) at the beginning of a game too.

def print_word_to_guess(letters):
    """Utility function to print the current word to guess"""
    print("Word to guess: {0}".format(" ".join(guess_word)))
Then we just print_word_to_guess(guess_word) whenever we need it. It would also be possible to make an optional different message but default to Word to guess:, but I'll leave that for you as a challenge.

The game also doesn't ever tell me anything about how many chances I have left, so I'm left guessing (literally) until I figure it out. That's very easy to make a small utility function to do:

def print_guesses_taken(current, total):
    """Prints how many chances the player has used"""
    print("You are on guess {0}/{1}.".format(current, total))
Then a few code additions:

def guessing():
    guess_taken = 1
    MAX_GUESS = 10
    print_guesses_taken(guess_taken, MAX_GUESS)

    while guess_taken < MAX_GUESS:
And:

for i in range(0, length_word):
    if secretWord[i] == guess:
        guess_word[i] = guess
print_word_to_guess(guess_word)
print_guesses_taken(guess_taken, MAX_GUESS)
Your list of words is pretty limited, for future consider perhaps looking for a text file online with a whole bunch of words and just read a random one from it, that would bring more variety!

Code Improvements
main function
You run all your game functions as soon as you make them. It would make more sense to put them inside the __main__ function:

if __name__ == "__main__":
    beginning()
    newFunc()
    change()
    guessing()
By the way, newFunc() doesn't work well as a name, as it says nothing about what it does. Something like ask_user_to_play() would be much better.

Constants naming
Python doesn't have real constants, but nevertheless, it's good practice to name variables that shouldn't change (by change I mean reassigned to a different values) in ALL_CAPS_WITH_UNDERSCORES. A simple find & replace in an IDE or text editor does the trick to fix your whole script.

GUESS_WORD = []
SECRET_WORD = random.choice(wordList) # lets randomize single word from the list
LENGTH_WORD = len(SECRET_WORD)
ALPHABET = "abcdefghijklmnopqrstuvwxyz"
letter_storage = []
Docstring
It's a good habit to add a docstring to all functions, classes, and modules, to describe for other programmers what the code in question is for. I've done that for the above 2 utility function.

Type hints : 
Since Python 3, you can now use type hints for your function and method signatures, and use static code analysis tools. This also makes the code easier to read for humans.

