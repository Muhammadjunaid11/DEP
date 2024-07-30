#Red Blue Nim Game 
#Submitted By
#Muhammad Junaid
#CNIC:37202-2283389-1
import tkinter as tk
from tkinter import messagebox

# Class to represent the Red-Blue Nim game
class RedBlueNim:
    def __init__(self, num_red, num_blue, version, first_player, depth):
        #intializing game intial parameters
        self.num_red = num_red
        self.num_blue = num_blue
        self.version = version
        self.first_player = first_player
        self.depth = depth
        self.current_player = first_player
# function to  Check if the game is over (when either pile is empty)
    def is_game_over(self):
        return self.num_red == 0 or self.num_blue == 0

    def make_move(self, color, amount):
        # Make a move by reducing the number of marbles of the given color
        if color == 'red':
            if self.num_red >= amount:
                self.num_red -= amount
                return True
        elif color == 'blue':
            if self.num_blue >= amount:
                self.num_blue -= amount
                return True
        return False

    def get_score(self):
         # Calculates the score based on the remaining marbles
        return self.num_red * 2 + self.num_blue * 3

    def switch_player(self):
        # Switch the current player
        self.current_player = 'human' if self.current_player == 'computer' else 'computer'
# Using MinMax algorithm with Alpha Beta Pruning for AI decision-making
def minmax(game, depth, alpha, beta, maximizing_player):
    if game.is_game_over() or depth == 0:
        score = game.get_score()
        if game.version == 'misere' and game.is_game_over():
            return -score if maximizing_player else score
        return score if maximizing_player else -score

    if maximizing_player:
        max_eval = float('-inf')
        for color in ['red', 'blue']:
            for amount in [1, 2]:
                new_game = RedBlueNim(game.num_red, game.num_blue, game.version, game.current_player, game.depth)
                if new_game.make_move(color, amount):
                    new_game.switch_player()
                    eval = minmax(new_game, depth-1, alpha, beta, False)
                    max_eval = max(max_eval, eval)
                    alpha = max(alpha, eval)
                    if beta <= alpha:
                        break
        return max_eval
    else:
        min_eval = float('inf')
        for color in ['red', 'blue']:
            for amount in [1, 2]:
                new_game = RedBlueNim(game.num_red, game.num_blue, game.version, game.current_player, game.depth)
                if new_game.make_move(color, amount):
                    new_game.switch_player()
                    eval = minmax(new_game, depth-1, alpha, beta, True)
                    min_eval = min(min_eval, eval)
                    beta = min(beta, eval)
                    if beta <= alpha:
                        break
        return min_eval

# Function to determine the best move for the computer
def computer_move(game):
    best_move = None
    best_value = float('-inf')
    for color in ['red', 'blue']:
        for amount in [1, 2]:
            new_game = RedBlueNim(game.num_red, game.num_blue, game.version, game.current_player, game.depth)
            if new_game.make_move(color, amount):
                new_game.switch_player()
                move_value = minmax(new_game, game.depth-1, float('-inf'), float('inf'), False)
                if move_value > best_value:
                    best_value = move_value
                    best_move = (color, amount)
    return best_move
# GUI class for the Red-Blue Nim game
class NimGameGUI:
    def __init__(self, root):
        self.root = root
        self.root.title("Red-Blue Nim Game")

        # Seting up game parameters
        self.setup_game()

    def setup_game(self):
        # Geting game parameters from the user
        self.num_red = int(input("Enter number of red marbles: ").strip())
        self.num_blue = int(input("Enter number of blue marbles: ").strip())
        self.version = input("Enter version (standard/misere, default is 'standard'): ").strip().lower() or 'standard'
        self.first_player = input("Enter first player (computer/human, default is 'computer'): ").strip().lower() or 'computer'
        self.depth = int(input("Enter search depth for AI (default is 3): ").strip() or 3)

        self.game = RedBlueNim(self.num_red, self.num_blue, self.version, self.first_player, self.depth)
        self.create_widgets()
        self.update_status()

    def create_widgets(self):
        self.status_label = tk.Label(self.root, text="", font=('Helvetica', 14))
        self.status_label.pack(pady=10)

        self.red_label = tk.Label(self.root, text="Red marbles: 0", font=('Helvetica', 12))
        self.red_label.pack()

        self.blue_label = tk.Label(self.root, text="Blue marbles: 0", font=('Helvetica', 12))
        self.blue_label.pack()

        self.move_frame = tk.Frame(self.root)
        self.move_frame.pack(pady=10)

        self.color_var = tk.StringVar(value='red')
        self.amount_var = tk.IntVar(value=1)

        self.red_radio = tk.Radiobutton(self.move_frame, text="Red", variable=self.color_var, value='red')
        self.red_radio.grid(row=0, column=0, padx=5)
        self.blue_radio = tk.Radiobutton(self.move_frame, text="Blue", variable=self.color_var, value='blue')
        self.blue_radio.grid(row=0, column=1, padx=5)

        self.amount_label = tk.Label(self.move_frame, text="Amount:")
        self.amount_label.grid(row=1, column=0, pady=5)

        self.amount_spinbox = tk.Spinbox(self.move_frame, from_=1, to=2, textvariable=self.amount_var)
        self.amount_spinbox.grid(row=1, column=1)

        self.move_button = tk.Button(self.root, text="Make Move", command=self.make_human_move)
        self.move_button.pack(pady=10)

    def update_status(self):
        self.red_label.config(text=f"Red marbles: {self.game.num_red}")
        self.blue_label.config(text=f"Blue marbles: {self.game.num_blue}")
        if self.game.current_player == 'human':
            self.status_label.config(text="Your turn!")
        else:
            self.status_label.config(text="Computer's turn")
            self.root.after(1000, self.make_computer_move)

    def make_human_move(self):
        color = self.color_var.get()
        amount = self.amount_var.get()
        if self.game.make_move(color, amount):
            self.game.switch_player()
            self.update_status()
            if self.game.is_game_over():
                self.end_game()
        else:
            messagebox.showerror("Invalid move", "You cannot make this move. Try again.")

    def make_computer_move(self):
        move = computer_move(self.game)
        if move:
            color, amount = move
            self.game.make_move(color, amount)
            messagebox.showinfo("Computer move", f"Computer took {amount} {color} marbles.")
            self.game.switch_player()
            self.update_status()
            if self.game.is_game_over():
                self.end_game()
        else:
            messagebox.showerror("Invalid move", "Computer cannot make a move. Game over.")

    def end_game(self):
        score = self.game.get_score()
        messagebox.showinfo("Game Over", f"Game over! Final score: {score}")
        self.root.quit()

def main():
    root = tk.Tk()
    app = NimGameGUI(root)
    root.mainloop()

if __name__ == "__main__":
    main()
