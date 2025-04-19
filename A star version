import heapq
import copy
import time
import tracemalloc

def is_valid(board, row, col, num):
    for i in range(9):
        if board[row][i] == num or board[i][col] == num:
            return False
    box_row = row - row % 3
    box_col = col - col % 3
    for i in range(box_row, box_row + 3):
        for j in range(box_col, box_col + 3):
            if board[i][j] == num:
                return False
    return True

def get_possible_digits(board, row, col):
    if board[row][col] != 0:
        return []
    possible = set(range(1, 10))
    for i in range(9):
        possible.discard(board[row][i])
        possible.discard(board[i][col])
    box_row = row - row % 3
    box_col = col - col % 3
    for i in range(box_row, box_row + 3):
        for j in range(box_col, box_col + 3):
            possible.discard(board[i][j])
    return list(possible)

def heuristic(board):
    h = 0
    for i in range(9):
        for j in range(9):
            if board[i][j] == 0:
                possible = get_possible_digits(board, i, j)
                if not possible:
                    return float('inf')  # Dead end
                h += len(possible)
    return h

def filled_count(board):
    return sum(1 for row in board for cell in row if cell != 0)

def find_most_constrained_cell(board):
    min_options = 10
    best_cell = None
    for i in range(9):
        for j in range(9):
            if board[i][j] == 0:
                options = get_possible_digits(board, i, j)
                if len(options) < min_options:
                    min_options = len(options)
                    best_cell = (i, j)
    return best_cell

def a_star_sudoku_solver(start_board):
    tracemalloc.start()
    start_time = time.time()
    steps = 0

    open_set = []
    heapq.heappush(open_set, (heuristic(start_board), filled_count(start_board), start_board))

    while open_set:
        f, g, board = heapq.heappop(open_set)
        steps += 1

        if heuristic(board) == 0:
            end_time = time.time()
            mem_current, mem_peak = tracemalloc.get_traced_memory()
            tracemalloc.stop()
            return {
                "solution": board,
                "steps": steps,
                "time": end_time - start_time,
                "memory_peak_kb": mem_peak / 1024
            }

        pos = find_most_constrained_cell(board)
        if not pos:
            continue

        row, col = pos
        for num in get_possible_digits(board, row, col):
            new_board = copy.deepcopy(board)
            new_board[row][col] = num

            h = heuristic(new_board)
            if h == float('inf'):
                continue

            new_g = g + 1
            heapq.heappush(open_set, (new_g + h, new_g, new_board))

    tracemalloc.stop()
    return {
        "solution": None,
        "steps": steps,
        "time": time.time() - start_time,
        "memory_peak_kb": 0
    }
    

# Test it out
puzzle = [
    [8, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 3, 6, 0, 0, 0, 0, 0],
    [0, 7, 0, 0, 9, 0, 2, 0, 0],
    [0, 5, 0, 0, 0, 7, 0, 0, 0],
    [0, 0, 0, 0, 4, 5, 7, 0, 0],
    [0, 0, 0, 1, 0, 0, 0, 3, 0],
    [0, 0, 1, 0, 0, 0, 0, 6, 8],
    [0, 0, 8, 5, 0, 0, 0, 1, 0],
    [0, 9, 0, 0, 0, 0, 4, 0, 0]
]

result = a_star_sudoku_solver(puzzle)

if result["solution"]:
    print("Solved Sudoku:")
    for row in result["solution"]:
        print(row)
    print("\nStats:")
    print(f"Steps taken       : {result['steps']}")
    print(f"Time taken (sec)  : {result['time']:.4f}")
    print(f"Peak memory (KB)  : {result['memory_peak_kb']:.2f}")
    
else:
    print("No solution found.")
