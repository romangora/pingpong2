import pygame
from pygame import *

# класс спрайтов
class Sprites(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed, wight, height):
        self.image = transform.scale(image.load(player_image), (wight, height))        # параметры
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))              #отрисовка


class Player(Sprites):
    def buttons_r(self):
        keys = key.get_pressed()                  #кнопки
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < w_height - 80:
            self.rect.y += self.speed

    def buttons_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < w_height - 80:
            self.rect.y += self.speed


# сцена:
TITLE = 'Ping Pong'
back = (0, 0, 0)  # цвет фона
w_width = 600
w_height = 400
window = display.set_mode((w_width, w_height))
window.fill(back)
w = display.set_caption(TITLE)

game = True
finish = False
clock = time.Clock()
FPS = 60

# мяч и ракетки
racket1 = Player('racket1.png', 30, 200, 4, 50, 150)
racket2 = Player('racket2.png', 520, 200, 4, 50, 150)
ball = Sprites('pingpongball.png', 200, 200, 4, 50, 50)

font.init()
font = font.Font(None, 35)
end1 = font.render('СИНИЙ ПРОИГРАЛ!', True, (255, 255, 255))
end2 = font.render('КРАСНЫЙ ПРОИГРАЛ!', True, (255, 255, 255))
speed_x = 5
speed_y = 5

while game:
    for event in pygame.event.get():            #выход
        if event.type == pygame.QUIT:
            game = False


    if finish != True:
        window.fill(back)                         #пока игра не закончится оно должно работать
        racket1.buttons_l()
        racket2.buttons_r()
        ball.rect.x += speed_x
        ball.rect.y += speed_y

        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):    #столкновение
            speed_x *= -1
            speed_y *= 1

        # смена направления мяча если коснулся границ экрана
        if ball.rect.y > w_height - 50 or ball.rect.y < 0:
            speed_y *= -1

        #вывод проигрыша для игроков
        if ball.rect.x < 0:
            finish = True
            window.blit(end1, (200, 200))
            game_over = True

        if ball.rect.x > w_width:
            finish = True
            window.blit(end2, (200, 200))
            game_over = True

        racket1.reset()           #возврат в исходное положение
        racket2.reset()
        ball.reset()

    clock.tick(FPS)
    display.update()
