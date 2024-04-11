Hangman Game README

Introduction

This project is a collaborative effort by Gurjot, Gurleen, and Divyansh, where Divyansh served as the manager, Gurleen as the developer, and Gurjot as the tester. The Hangman Game is implemented in Python using the Tkinter library for the graphical user interface.

Versions Overview

Version 1

Code:

import tkinter as tk
from tkinter import messagebox
import random

class HangmanGame:
    def __init__(self, master):
        self.master = master
        self.master.title("Hangman Game")
        self.master.geometry("400x400")

        self.word_list = {
            "easy": ["apple", "banana", "carrot", "dog", "elephant", "flower", "sun", "moon", "star", "tree"],
            "medium": ["computer", "python", "programming", "internet", "keyboard", "algorithm", "hangman", "puzzle", "coding", "rocket"],
            "hard": ["exaggerate", "juxtaposition", "equilibrium", "quintessential", "paradox", "ineffable", "cryptocurrency", "extraterrestrial", "boulevard", "incomprehensible"]
        }

        self.level = tk.StringVar(master)
        self.level.set("easy")
        self.level_label = tk.Label(master, text="Select Level:")
        self.level_label.pack()
        self.level_menu = tk.OptionMenu(master, self.level, "easy", "medium", "hard", command=self.start_game)
        self.level_menu.pack()

        self.word_to_guess = ""
        self.word_display = ""
        self.attempts = 7
        self.guesses = []
        self.wins = 0
        self.losses = 0

        self.canvas = tk.Canvas(master, width=200, height=200)
        self.canvas.pack()

        self.entry_label = tk.Label(master, text="Guess a letter:")
        self.entry_label.pack()
        self.entry_var = tk.StringVar()
        self.entry = tk.Entry(master, textvariable=self.entry_var, width=5)
        self.entry.pack()

        self.guess_button = tk.Button(master, text="Guess", command=self.guess_letter)
        self.guess_button.pack()

        self.restart_button = tk.Button(master, text="Restart", command=self.start_game)
        self.restart_button.pack()

        self.reveal_button = tk.Button(master, text="Reveal", command=self.reveal_word)
        self.reveal_button.pack()
        self.reveal_button.config(state="disabled")

        self.start_game()

    def start_game(self, *args):
        self.attempts = 7
        self.guesses = []
        self.word_to_guess = random.choice(self.word_list[self.level.get()])
        self.word_display = "_" * len(self.word_to_guess)
        self.display_word()
        self.draw_hangman()

    def display_word(self):
        self.canvas.delete("word")
        for i, letter in enumerate(self.word_display):
            self.canvas.create_text(50 + i * 20, 150, text=letter, font=("Helvetica", 16), tag="word")

    def guess_letter(self):
        letter = self.entry_var.get().lower()
        self.entry_var.set("")  # Clear entry after guessing
        if letter.isalpha() and len(letter) == 1:
            if letter not in self.guesses:
                self.guesses.append(letter)
                if letter in self.word_to_guess:
                    temp_word = ""
                    for i in range(len(self.word_to_guess)):
                        if self.word_to_guess[i] == letter:
                            temp_word += letter
                        else:
                            temp_word += self.word_display[i]
                    self.word_display = temp_word
                    if "_" not in self.word_display:
                        messagebox.showinfo("Congratulations!", "You've guessed the word!")
                        self.wins += 1
                        self.update_scoreboard()
                        self.start_game()
                else:
                    self.attempts -= 1
                    self.draw_hangman()
                    if self.attempts == 0:
                        messagebox.showinfo("Game Over", f"You've run out of attempts! The word was {self.word_to_guess}.")
                        self.losses += 1
                        self.update_scoreboard()
                        self.reveal_word()
                        self.start_game()
                self.display_word()
                self.update_guesses_display()
            else:
                messagebox.showinfo("Invalid Guess", "You've already guessed this letter.")
        else:
            messagebox.showinfo("Invalid Input", "Please enter a single letter.")

    def draw_hangman(self):
        self.canvas.delete("hangman")
        if self.attempts < 7:
            parts = 7 - self.attempts
            if parts == 1:  # Head
                self.canvas.create_oval(40, 40, 80, 80, tag="hangman", width=3)
            elif parts == 2:  # Body
                self.canvas.create_line(60, 80, 60, 150, tag="hangman", width=3)
            elif parts == 3:  # Left Arm
                self.canvas.create_line(60, 100, 30, 110, tag="hangman", width=3)
            elif parts == 4:  # Right Arm
                self.canvas.create_line(60, 100, 90, 110, tag="hangman", width=3)
            elif parts == 5:  # Left Leg
                self.canvas.create_line(60, 150, 30, 180, tag="hangman", width=3)
            elif parts == 6:  # Right Leg
                self.canvas.create_line(60, 150, 90, 180, tag="hangman", width=3)

    def update_guesses_display(self):
        guessed_letters = ", ".join(self.guesses)
        guessed_label = tk.Label(self.master, text=f"Guessed Letters: {guessed_letters}")
        guessed_label.pack_forget()
        guessed_label.pack()

    def update_scoreboard(self):
        score_label = tk.Label(self.master, text=f"Wins: {self.wins}  |  Losses: {self.losses}")
        score_label.pack_forget()
        score_label.pack()

    def reveal_word(self):
        messagebox.showinfo("Word Revealed", f"The word was: {self.word_to_guess}")

def main():
    root = tk.Tk()
    game = HangmanGame(root)
    root.mainloop()

if __name__ == "__main__":
    main()


Key Features:
•	Implemented using Tkinter for GUI.
•	Three levels of difficulty: easy, medium, and hard.
•	Displays hangman figure using canvas.
•	Shows guessed letters and updates the scoreboard.
Version 2
Code:
import tkinter as tk
from tkinter import messagebox
import random

class HangmanGame:
    def __init__(self, master):
        self.master = master
        self.master.title("Hangman Game")
        self.master.geometry("400x400")

        self.word_list = {
            "easy": ["apple", "banana", "carrot", "dog", "elephant", "flower", "sun", "moon", "star", "tree"],
            "medium": ["computer", "python", "programming", "internet", "keyboard", "algorithm", "hangman", "puzzle", "coding", "rocket"],
            "hard": ["exaggerate", "juxtaposition", "equilibrium", "quintessential", "paradox", "ineffable", "cryptocurrency", "extraterrestrial", "boulevard", "incomprehensible"]
        }

        self.level = tk.StringVar(master)
        self.level.set("easy")
        self.level_label = tk.Label(master, text="Select Level:")
        self.level_label.pack()
        self.level_menu = tk.OptionMenu(master, self.level, "easy", "medium", "hard", command=self.start_game)
        self.level_menu.pack()

        self.word_to_guess = ""
        self.word_display = ""
        self.attempts = 7
        self.guesses = []

        self.canvas = tk.Canvas(master, width=200, height=200)
        self.canvas.pack()

        self.entry_label = tk.Label(master, text="Guess a letter:")
        self.entry_label.pack()
        self.entry_var = tk.StringVar()
        self.entry = tk.Entry(master, textvariable=self.entry_var, width=5)
        self.entry.pack()

        self.guess_button = tk.Button(master, text="Guess", command=self.guess_letter)
        self.guess_button.pack()

        self.restart_button = tk.Button(master, text="Restart", command=self.start_game)
        self.restart_button.pack()

        self.start_game()

    def start_game(self, *args):
        self.attempts = 7
        self.guesses = []
        self.word_to_guess = random.choice(self.word_list[self.level.get()])
        self.word_display = "_" * len(self.word_to_guess)
        self.display_word()
        self.draw_hangman()

    def display_word(self):
        self.canvas.delete("word")
        for i, letter in enumerate(self.word_display):
            self.canvas.create_text(50 + i * 20, 150, text=letter, font=("Helvetica", 16), tag="word")

    def guess_letter(self):
        letter = self.entry_var.get().lower()
        self.entry_var.set("")  # Clear entry after guessing
        if letter.isalpha() and len(letter) == 1:
            if letter not in self.guesses:
                self.guesses.append(letter)
                if letter in self.word_to_guess:
                    temp_word = ""
                    for i in range(len(self.word_to_guess)):
                        if self.word_to_guess[i] == letter:
                            temp_word += letter
                        else:
                            temp_word += self.word_display[i]
                    self.word_display = temp_word
                    if "_" not in self.word_display:
                        messagebox.showinfo("Congratulations!", "You've guessed the word!")
                        self.start_game()
                else:
                    self.attempts -= 1
                    self.draw_hangman()
                    if self.attempts == 0:
                        messagebox.showinfo("Game Over", f"You've run out of attempts! The word was {self.word_to_guess}.")
                        self.start_game()
                self.display_word()
            else:
                messagebox.showinfo("Invalid Guess", "You've already guessed this letter.")
        else:
            messagebox.showinfo("Invalid Input", "Please enter a single letter.")

    def draw_hangman(self):
        self.canvas.delete("hangman")
        if self.attempts < 7:
            parts = 7 - self.attempts
            if parts == 1:  # Head
                self.canvas.create_oval(40, 40, 80, 80, tag="hangman", width=3)
            elif parts == 2:  # Body
                self.canvas.create_line(60, 80, 60, 150, tag="hangman", width=3)
            elif parts == 3:  # Left Arm
                self.canvas.create_line(60, 100, 30, 110, tag="hangman", width=3)
            elif parts == 4:  # Right Arm
                self.canvas.create_line(60, 100, 90, 110, tag="hangman", width=3)
            elif parts == 5:  # Left Leg
                self.canvas.create_line(60, 150, 30, 180, tag="hangman", width=3)
            elif parts == 6:  # Right Leg
                self.canvas.create_line(60, 150, 90, 180, tag="hangman", width=3)
                

def main():
    root = tk.Tk()
    game = HangmanGame(root)
    root.mainloop()

if __name__ == "__main__":
    main()


Key Features:
•	Similar to Version 1 but with some modifications.
•	Removed the scoreboard display.
•	Streamlined the user interface.
•	Removed the feature to reveal the word.
Version 3
Code:
import tkinter as tk
from tkinter import messagebox
import random
from PIL import Image, ImageTk
class HangmanGame:
    def __init__(self, master):
        self.master = master
        self.master.title("Hangman Game")
        self.master.geometry("400x450")

        self.word_list = {
            "easy": ["apple", "banana", "carrot", "dog", "elephant", "flower", "sun", "moon", "star", "tree"],
            "medium": ["computer", "python", "programming", "internet", "keyboard", "algorithm", "hangman", "puzzle", "coding", "rocket"],
            "hard": ["exaggerate", "juxtaposition", "equilibrium", "quintessential", "paradox", "ineffable", "cryptocurrency", "extraterrestrial", "boulevard", "incomprehensible"]
        }

        self.level = tk.StringVar(master)
        self.level.set("easy")
        self.level_label = tk.Label(master, text="Select Level:")
        self.level_label.pack()
        self.level_menu = tk.OptionMenu(master, self.level, "easy", "medium", "hard", command=self.start_game)
        self.level_menu.pack()

        self.word_to_guess = ""
        self.word_display = ""
        self.attempts = 7
        self.guesses = []
        self.wins = 0
        self.losses = 0

        self.canvas = tk.Canvas(master, width=200, height=200)
        self.canvas.pack()

        self.entry_label = tk.Label(master, text="Guess a letter:")
        self.entry_label.pack()
        self.entry_var = tk.StringVar()
        self.entry = tk.Entry(master, textvariable=self.entry_var, width=5)
        self.entry.pack()

        self.guess_button = tk.Button(master, text="Guess", command=self.guess_letter)
        self.guess_button.pack()

        self.restart_button = tk.Button(master, text="Restart", command=self.start_game)
        self.restart_button.pack()

        self.reveal_button = tk.Button(master, text="Reveal", command=self.reveal_word)
        self.reveal_button.pack()
        self.reveal_button.config(state="disabled")

        self.load_images()
        self.start_game()

    def load_images(self):
        self.hangman_images = [Image.open(f"hangman.jpeg") for i in range(7)]
        self.hangman_images = [ImageTk.PhotoImage(image) for image in self.hangman_images]

    def start_game(self, *args):
        self.attempts = 7
        self.guesses = []
        self.word_to_guess = random.choice(self.word_list[self.level.get()])
        self.word_display = "_" * len(self.word_to_guess)
        self.display_word()
        self.draw_hangman()
        self.reveal_button.config(state="normal")

    def display_word(self):
        self.canvas.delete("word")
        for i, letter in enumerate(self.word_display):
            self.canvas.create_text(50 + i * 20, 150, text=letter, font=("Helvetica", 16), tag="word")

    def guess_letter(self):
        letter = self.entry_var.get().lower()
        self.entry_var.set("")  # Clear entry after guessing
        if letter.isalpha() and len(letter) == 1:
            if letter not in self.guesses:
                self.guesses.append(letter)
                if letter in self.word_to_guess:
                    temp_word = ""
                    for i in range(len(self.word_to_guess)):
                        if self.word_to_guess[i] == letter:
                            temp_word += letter
                        else:
                            temp_word += self.word_display[i]
                    self.word_display = temp_word
                    if "_" not in self.word_display:
                        messagebox.showinfo("Congratulations!", "You've guessed the word!")
                        self.wins += 1
                        self.update_scoreboard()
                        self.start_game()
                else:
                    self.attempts -= 1
                    self.draw_hangman()
                    if self.attempts == 0:
                        messagebox.showinfo("Game Over", f"You've run out of attempts! The word was {self.word_to_guess}.")
                        self.losses += 1
                        self.update_scoreboard()
                        self.reveal_word()
                        self.start_game()
                self.display_word()
                self.update_guesses_display()
            else:
                messagebox.showinfo("Invalid Guess", "You've already guessed this letter.")
        else:
            messagebox.showinfo("Invalid Input", "Please enter a single letter.")

    def draw_hangman(self):
        self.canvas.delete("hangman")
        if self.attempts < 7:
            self.canvas.create_image(100, 100, anchor=tk.CENTER, image=self.hangman_images[7 - self.attempts], tag="hangman")

    def update_guesses_display(self):
        guessed_letters = ", ".join(self.guesses)
        guessed_label = tk.Label(self.master, text=f"Guessed Letters: {guessed_letters}")
        guessed_label.pack_forget()
        guessed_label.pack()

    def update_scoreboard(self):
        score_label = tk.Label(self.master, text=f"Wins: {self.wins}  |  Losses: {self.losses}")
        score_label.pack_forget()
        score_label.pack()

    def reveal_word(self):
        messagebox.showinfo("Word Revealed", f"The word was: {self.word_to_guess}")

def main():
    root = tk.Tk()
    game = HangmanGame(root)
    root.mainloop()

if __name__ == "__main__":
    main()


Key Features:
•	Enhanced GUI with hangman images.
•	Added functionality to reveal the word.
•	Shows wins and losses in the scoreboard.
•	Improved user experience with visual representation.
Differences between Versions
•	Version 1: Basic implementation with a focus on functionality. It includes all essential features such as level selection, hangman drawing, and guessing functionality.
•	Version 2: Streamlined version with improvements in user interface and user experience. It removes some features like the scoreboard for simplicity.
•	Version 3: Enhanced version with graphical improvements. It includes hangman images for a more engaging experience, along with added features like revealing the word and an improved scoreboard.
Conclusion
The Hangman Game project evolved through three versions, each focusing on different aspects such as functionality, user interface, and graphical enhancements. With contributions from Divyansh, Gurleen, and Gurjot, the project demonstrates teamwork and collaboration in software development.
