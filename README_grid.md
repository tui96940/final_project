#grid.py file#
from random import sample
from selection import SelectNumber
from copy import deepcopy

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

def remove_numbers(grid: list[list]) -> None:
    #randomly sets numbers to zero on the grid
    num_of_cells = GRID_SIZE * GRID_SIZE
    empties = num_of_cells * 3 // 7  #if you want to make the game harder lower this number to 4
    for i in sample(range(num_of_cells), empties):
        grid[i//GRID_SIZE][i%GRID_SIZE]=0

class Grid:
    def __init__(self, pygame, font):
        self.cellSize=50
        self.num_x_offset=15
        self.num_y_offset=8
        self.line_coordinates=create_line_coordinates(self.cellSize)
        self.win=False
        self.game_font = font

        self.grid = create_grid(SUB_GRID_SIZE)
        self.__test_grid=deepcopy(self.grid)       
        remove_numbers(self.grid)
        self.occupied_cell_coordinates=self.pre_occupied_cells()
        #print(self.occupied_cell_coordinates)

        self.selection = SelectNumber(pygame, self.game_font)

    def restart(self) -> None:
        self.grid = create_grid(SUB_GRID_SIZE)
        self.__test_grid=deepcopy(self.grid)
        remove_numbers(self.grid)
        self.occupied_cell_coordinates=self.pre_occupied_cells()
        self.win=False

    def check_grids(self):
        for y in range(len(self.grid)):
            for x in range(len(self.grid[y])):
                if self.grid[y][x] != self.__test_grid[y][x]:
                    return False
        return True
                

    def is_cell_preoccupied(self, x: int, y: int) -> bool:
        for cell in self.occupied_cell_coordinates:
            if x == cell[1] and y == cell[0]: #x is column, y is row
                return True
        return False

    def get_mouse_click(self, x: int, y: int) -> None:
        if x<=450:
            grid_x, grid_y = x // 50, y // 50
            #print(grid_x, grid_y)
            if not self.is_cell_preoccupied(grid_x, grid_y):
                self.set_cell(grid_x, grid_y, self.selection.selected_number)
        self.selection.button_clicked(x, y)
        if self.check_grids():
            print("You won, game over!")
            self.win=True

    def pre_occupied_cells(self) -> list[tuple]:
        occupied_cell_coordinates=[]
        for y in range(len(self.grid)):
            for x in range(len(self.grid[y])):
                if self.get_cell(x, y) != 0:
                    occupied_cell_coordinates.append((y,x))
        return occupied_cell_coordinates

    def __draw_lines(self, pg, surface)-> None:
        for index, point in enumerate(self.line_coordinates):
            if index == 2 or index == 5 or index ==10 or index ==13:
                pg.draw.line(surface, (255, 200, 0), point[0], point[1])
            else:
                pg.draw.line(surface, (0,25,0), point[0], point[1])
    def __draw_numbers(self, surface) -> None:
        #draw the grid numbers
        for y in range(len(self.grid)):
            for x in range(len(self.grid[y])):
                if self.get_cell(x, y) != 0:
                    if (y, x) in self.occupied_cell_coordinates:
                        text_surface = self.game_font.render(str(self.get_cell(x, y)), False, (0, 200, 255))
                    else:
                        text_surface = self.game_font.render(str(self.get_cell(x, y)), False, (0, 255, 0))
                    if self.get_cell(x,y) != self.__test_grid[y][x]:
                        text_surface = self.game_font.render(str(self.get_cell(x, y)), False, (255, 0, 0))
                        
                    surface.blit(text_surface,(x * self.cellSize + self.num_x_offset, y * self.cellSize + self.num_y_offset))

    def draw_all(self, pg, surface):
        self.__draw_lines(pg, surface)
        self.__draw_numbers(surface)
        self.selection.draw(pg, surface)

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
