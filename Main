import random
import os
import time


BOARD_SIZE = 7
SHIP_SIZES = [3, 2, 2, 1, 1, 1, 1]
SHIP_NAMES = {3: "Battleship", 2: "Cruiser", 1: "Destroyer"}


def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

# Function to create an empty board
def create_board(size):
    return [[" " for _ in range(size)] for _ in range(size)]


def print_board(board, show_ships=False):
    header = "  " + " ".join([chr(c) for c in range(ord('A'), ord('A') + BOARD_SIZE)])
    print(header)
    for idx, row in enumerate(board):
        row_str = str(idx + 1) + " " + " ".join(row)
        if not show_ships:
            row_str = row_str.replace("S", " ")
        print(row_str)


def place_ships(board, ship_sizes):
    for size in ship_sizes:
        placed = False
        while not placed:
            orientation = random.choice(['horizontal', 'vertical'])
            if orientation == 'horizontal':
                row = random.randint(0, BOARD_SIZE - 1)
                col = random.randint(0, BOARD_SIZE - size)
                if all(board[row][col+i] == " " for i in range(size)):
                    for i in range(size):
                        board[row][col+i] = "S"
                    placed = True
            else:
                row = random.randint(0, BOARD_SIZE - size)
                col = random.randint(0, BOARD_SIZE - 1)
                if all(board[row+i][col] == " " for i in range(size)):
                    for i in range(size):
                        board[row+i][col] = "S"
                    placed = True


def get_coordinates():
    while True:
        try:
            shot = input("Enter coordinates for your shot (e.g., A1): ").upper()
            if len(shot) < 2 or not shot[0].isalpha() or not shot[1:].isdigit():
                raise ValueError("Invalid input, please use the format 'A1'.")
            if ord(shot[0]) - ord('A') >= BOARD_SIZE or int(shot[1:]) - 1 >= BOARD_SIZE:
                raise ValueError("Shot out of the battlefield.")
            return ord(shot[0]) - ord('A'), int(shot[1:]) - 1
        except ValueError as e:
            print(e)


def take_shot(board, row, col):
    if board[row][col] == " ":
        return "miss"
    elif board[row][col] == "S":
        board[row][col] = "X"
        if is_sunk(board, row, col):
            return "sunk"
        return "hit"
    return "repeat"


def is_sunk(board, row, col):
    ship_cells = [(row, col)]
    # Check horizontally
    for i in range(col-1, -1, -1):
        if board[row][i] == "X":
            ship_cells.append((row, i))
        else:
            break
    for i in range(col+1, BOARD_SIZE):
        if board[row][i] == "X":
            ship_cells.append((row, i))
        else:
            break
    
    for i in range(row-1, -1, -1):
        if board[i][col] == "X":
            ship_cells.append((i, col))
        else:
            break
    for i in range(row+1, BOARD_SIZE):
        if board[i][col] == "X":
            ship_cells.append((i, col))
        else:
            break
    # If all cells of the ship are hit, it's sunk
    return all(board[r][c] == "X" for r, c in ship_cells)


def all_sunk(board):
    return all(cell != "S" for row in board for cell in row)


def game():
    player_name = input("Enter your name: ")
    board = create_board(BOARD_SIZE)
    place_ships(board, SHIP_SIZES)
    shots = 0
    while not all_sunk(board):
        print_board(board)
        row, col = get_coordinates()
        result = take_shot(board, row, col)
        if result != "repeat":
            shots += 1
        clear_screen()
        print(f"{result.capitalize()}!")
        if result == "sunk":
            print_board(board)
            time.sleep(2)
    print(f"Congratulations, {player_name}! You have sunk all the ships in {shots} shots.")
    return player_name, shots

def main():
    leaderboard = []
    while True:
        leaderboard.append(game())
        if input("Would you like to play again? (yes/no): ").lower() != "yes":
            break
    leaderboard.sort(key=lambda x: x[1])
    print("Leaderboard:")
    for name, score in leaderboard:
        print(f"{name}: {score} shots")
    print("Thank you for playing!")

if __name__ == "__main__":
    main()
