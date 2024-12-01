#grid.py file#
from random import sample

def create_line_coordinates(cellSize):
    #creates the x, y coordinated for drawing the grid lines
    points=[]
    for y in range(1,9):
        temp=[]
        temp.append((0, y*cellSize))
        temp.append((450, y*cellSize))
        points.append(temp)
    for x in range(1,10):
        temp=[]
        temp.append((x*cellSize, 0))
        temp.append((x*cellSize, 450))
        points.append(temp)
    print(points)
    return points

SUB_GRID_SIZE = 3
GRID_SIZE = SUB_GRID_SIZE * SUB_GRID_SIZE

def pattern(row_num: int, col_num: int) -> int:
    return (SUB_GRID_SIZE * (row_num % SUB_GRID_SIZE) + row_num // SUB_GRID_SIZE + col_num) % GRID_SIZE

def shuffle(samp: range) -> list:
    return sample(samp, len(samp))

def create_grid(sub_grid: int) -> list[list]:
    #creates 9x9 grid filled with random numbers
    row_base = range(sub_grid)
    rows = [g * sub_grid + r for g in shuffle(row_base) for r in shuffle(row_base)]
    cols = [g * sub_grid + c for g in shuffle(row_base) for c in shuffle(row_base)]
    nums = shuffle(range(1, sub_grid * sub_grid + 1))
    return [[nums[pattern(r, c)] for c in cols] for r in rows]

class Grid:
    def __init__(self, font):
        self.cellSize=50
        self.line_coordinates=create_line_coordinates(self.cellSize)
        self.grid = create_grid(SUB_GRID_SIZE)
        self.game_font = font
    def draw_lines(self, pg, surface)-> None:
        for index, point in enumerate(self.line_coordinates):
            if index == 2 or index == 5 or index ==10 or index ==13:
                pg.draw.line(surface, (255, 200, 0), point[0], point[1])
            else:
                pg.draw.line(surface, (0,25,0), point[0], point[1])
    def draw_numbers(self, surface) -> None:
        #draw the grid numbers
        for y in range(len(self.grid)):
            for x in range(len(self.grid[y])):
                text_surface = self.game_font.render(str(self.get_cell(x, y)), False, (0, 200, 255))
                surface.blit(text_surface,(x * self.cellSize, y * self.cellSize))

    def get_cell(self, x: int, y: int) -> int:
        return self.grid[y][x]

    def set_cell(self, x: int, y: int, value: int) -> None:
        self.grid[y][x]=value

    def show(self):
        for row in self.grid:
            print(row)

if __name__ == "__main__":
    grid=Grid()
    grid.show()

