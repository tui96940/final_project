#selection.py#
class SelectNumber:
    def __init__(self, pygame, font):
        self.pygame = pygame
        self.btn_w=40
        self.btn_h=40
        self.my_font=font
        self.selected_number=0

        self.color_selected=(0, 255, 0)
        self.color_normal = (200, 200, 200)

        self.btn_positions = [(475, 25), (525, 25),
                              (475, 75), (525, 75),
                              (475, 125), (525, 125),
                              (475, 175), (525, 175),
                              (525, 225)]

    def draw(self, pygame, surface):
        for index, pos in enumerate(self.btn_positions):
            pygame.draw.rect(surface, self.color_normal, [pos[0], pos[1], self.btn_w, self.btn_h], width=2, border_radius=5)

            if self.button_hover(pos):
                pygame.draw.rect(surface, self.color_selected, [pos[0], pos[1], self.btn_w, self.btn_h], width=2, border_radius=5)
                text_surface=self.my_font.render(str(index+1), False, (0, 255, 0))
            else:
                text_surface=self.my_font.render(str(index+1), False, self.color_normal)
            if self.selected_number >0:
                if self.selected_number - 1 == index:
                    pygame.draw.rect(surface, self.color_selected, [pos[0], pos[1], self.btn_w, self.btn_h], width=2, border_radius=5)
                    text_surface = self.my_font.render(str(index + 1), False, self.color_selected)

            surface.blit(text_surface, (pos[0] + 13, pos[1]))

    def button_clicked(self, mouse_x: int, mouse_y: int) -> None:
        for index, pos in enumerate(self.btn_positions):
            if self.on_button(mouse_x, mouse_y, pos):
                self.selected_number = index + 1

    def button_hover(self, pos: tuple) -> bool | None:
        mouse_pos=self.pygame.mouse.get_pos()
        if self.on_button(mouse_pos[0], mouse_pos[1], pos):
            return True

    def on_button(self, mouse_x: int, mouse_y: int, pos: tuple) -> bool:
        return mouse_x > pos[0] and mouse_x < pos[0] + self.btn_w and mouse_y > pos[1] and mouse_y < pos[1] + self.btn_h
    
