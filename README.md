#sudoku.py file#
import pygame
import os
from grid import Grid

os.environ['SDL_VIDEO_WINDEO_POS']="%d,%d"%(250,50)

surface = pygame.display.set_mode((600,450))#creates game window
pygame.display.set_caption("sudoku")

pygame.font.init()
game_font = pygame.font.SysFont('Comic Sans MS', 25)

grid = Grid(game_font)
running = True

while running:
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            runing = False
    surface.fill((0,0,0)) #makes window surface black

    grid.draw_lines(pygame, surface)
    grid.draw_numbers(surface)

    pygame.display.flip()

