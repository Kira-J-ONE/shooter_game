# shooter_game
#Создай собственный Шутер!
from pygame import *
from random import randint

window_width = 700
window_height = 500
window = display.set_mode((window_width, window_height))
display.set_caption('Shooter game')
background = transform.scale(image.load('galaxy.jpg'), (window_width, window_height))
mixer.init()
#mixer.music.load("Stray Kids - SLASH.mp3")
#mixer.music.play()
font.init()


class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (65, 85))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y
    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def __init__(self, player_image, player_x, player_y, player_speed):
        super().__init__(player_image, player_x, player_y, player_speed)
        self.lost = 0
        self.kills = 0
        self.wait = 0
    def update(self):
        keys = key.get_pressed()
        if keys[K_LEFT] and self.rect.x > 5:
            self.rect.x -=self.speed
        if keys[K_RIGHT] and self.rect.x < window_width - 80:
            self.rect.x += self.speed
    def fire(self):
        bullets = sprite.Group()
        bullets.add(bullet)

sprite1 = Player('2925559531.jpg',340,400,2)
font1 = font.SysFont('Arial',20)
sprite1.lost = 0
class Enemy(GameSprite):
    def respawn(self):
        self.rect.y = randint(0,1)
        self.rect.x = randint(50,650)
        self.speed = randint(1,2)
    def update(self):
        self.rect.y += self.speed
        if self.rect.y > window_height:
            self.respawn()
            sprite1.lost += 1
            text_lose = font1.render("You've lost:"+ str(sprite1.lost), 1,(255,255,255))

class Bullet(GameSprite):
    def update(self, sprite_x, sprite_y, sprite_center_x,sprite_top):
        sprite_x = Player.rect.x
        sprite_y = Player.rect.y
        sprite_center_x = Player.rect.centerx
        sprite_top = Player.rect.top
        keys = key.get_pressed()
        if keys[K_W]:
            self.rect.y -= self.speed

#bullet = Bullet(bullet.png, self.rect.centerx, self.rect.top, 15, 20, -15)

spriteF = Enemy('ufo.png',3,6,randint(1,3))
spriteE = Enemy('ufo.png',3,6,randint(1,3))
spriteL = Enemy('ufo.png',3,6,randint(1,3))
spriteI = Enemy('ufo.png',3,6,randint(1,3))
spriteX = Enemy('ufo.png',3,6,randint(1,3))

FPS = 100
run = True
clock = time.Clock()
while run:
    #bullet.update()

    sprite1.update()
    spriteF.update()
    spriteE.update()
    spriteL.update()
    spriteI.update()
    spriteX.update()
    for e in event.get():
        if e.type == QUIT:
            run = False
    
    window.blit(background,(0,0))
    #window.blit(text_lose,(0,0))

    sprite1.reset()
    spriteF.reset()
    spriteE.reset()
    spriteL.reset()
    spriteI.reset()
    spriteX.reset()
    display.update()
    clock.tick(FPS)















