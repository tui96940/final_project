#youtube link: https://www.youtube.com/watch?v=RMy66ja0cXA

#sudoku.py file#
import pygame
import os
from grid import Grid

os.environ['SDL_VIDEO_WINDEO_POS']="%d,%d"%(250,50)

surface = pygame.display.set_mode((600,450))#creates game window
pygame.display.set_caption("sudoku")

pygame.font.init()
game_font = pygame.font.SysFont('Comic Sans MS', 25)
game_font2=pygame.font.SysFont('Arial', 12)

grid = Grid(pygame, game_font)
running = True


while running:
    for event in pygame.event.get():
        if event.type==pygame.QUIT:
            runing = False
        if event.type == pygame.MOUSEBUTTONDOWN and not grid.win:
            if pygame.mouse.get_pressed()[0]:
                pos=pygame.mouse.get_pos()
                grid.get_mouse_click(pos[0], pos[1])
        if event.type==pygame.KEYDOWN:
            if event.key==pygame.K_RETURN and grid.win:
                    grid.restart()
                
    surface.fill((0,0,0)) #makes window surface black

    grid.draw_all(pygame, surface)

    if grid.win:
        won_surface=game_font.render("Yon Won!",False,(191, 64, 191))
        surface.blit(won_surface, (475,300))
        #press_space_surf1=game_font2.render("Press space for Easy!", False, (0,255,200))
        #surface.blit(press_space_surf1, (457, 350))
        press_space_surf2=game_font2.render("Press enter to restart!", False, (255,255,0))
        surface.blit(press_space_surf2, (475, 345))

    pygame.display.flip()

    
