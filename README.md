#sudoku.py file#
import pygame
import os
from grid import Grid

os.environ['SDL_VIDEO_WINDEO_POS']="%d,%d"%(250,50)

surface = pygame.display.set_mode((600,450))#creates game window
pygame.display.set_caption("sudoku")

pygame.font.init()
game_font = pygame.font.SysFont('Comic Sans MS', 25)

grid = Grid(pygame, game_font)
running = True

while running:
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            runing = False
        if event.type == pygame.MOUSEBUTTONDOWN:
            if pygame.mouse.get_pressed()[0]:
                pos=pygame.mouse.get_pos()
                grid.get_mouse_click(pos[0], pos[1])
                
    surface.fill((0,0,0)) #makes window surface black

    grid.draw_all(pygame, surface)

    pygame.display.flip()


