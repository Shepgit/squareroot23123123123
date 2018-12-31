import pygame,sys,random
pygame.mixer.init()
pygame.init()
pygame.font.init()
menu_music=pygame.mixer.music.load("menu.wav")
pygame.mixer.music.set_volume(0.10)
pygame.mixer.music.play(-1)
font = pygame.font.Font("ARCADECLASSIC.ttf",52)
clock = pygame.time.Clock()
DEFAULT_WIDTH=800
DEFAULT_HEIGHT=640
global BLACK,GRAY,WHITE,GREEN,RED,BLUE,TEXT_LIST,LEFT_MOUSE_BUTTON_CLICKED,RIGHT_MOUSE_BUTTON_CLICKED
RIGHT_MOUSE_BUTTON_CLICKED=0
LEFT_MOUSE_BUTTON_CLICKED=0
BLACK=1
WHITE=0
GREEN=2
RED=3
BLUE=4
GRAY=5
BUTTON_LIST=[]
class Button:
    def __init__(self,LOCATION,TEXT,DIMENSIONS,CLICKABLE,COLOUR,CLICKED):
        self.TEXT_STRING=TEXT
        self.TEXT=font.render(TEXT,True,(COLOUR))
        self.DIMENSIONS=([self.TEXT.get_width(),self.TEXT.get_height()])
        self.LOCATION=(400-self.DIMENSIONS[0]/2,LOCATION[1])
        self.CLICKABLE=CLICKABLE
        self.COLOUR=COLOUR
        self.CLICKED=0
    def draw(self):
        self.TEXT=font.render(self.TEXT_STRING,True,(self.COLOUR))
        GAME.DISPLAY.blit(self.TEXT,self.LOCATION)
    def detect_collision(self):
        if self.CLICKABLE == 1:
            if self.LOCATION[0] < pygame.mouse.get_pos()[0] < self.LOCATION[0] + self.DIMENSIONS[0] and self.LOCATION[1] < pygame.mouse.get_pos()[1] < self.LOCATION[1] + self.DIMENSIONS[1]:
                self.COLOUR=(200,195,194)
                print(LEFT_MOUSE_BUTTON_CLICKED)
                if LEFT_MOUSE_BUTTON_CLICKED==1:
                    self.sort_button()
            else:
                self.COLOUR=(255,255,255)
    def sort_button(self):
        if self.TEXT_STRING == 'PLAY':
            GAME.main()
        elif self.TEXT_STRING == 'OPTIONS':
            GAME.options()
        elif self.TEXT_STRING == 'QUIT':
            pygame.quit()
            sys.exit()
        elif self.TEXT_STRING == 'BACK':
            GAME.main_menu()
BUTTON_PLAY=Button([0,150],'PLAY',[0,0],1,(255,255,255),0)
BUTTON_QUIT=Button([0,250],'QUIT',[0,0],1,(255,255,255),0)
BUTTON_OKAY=Button([0,200],'OKAY',[0,0],1,(255,255,255),0)
BUTTON_OPTIONS=Button([0,200],'OPTIONS',[0,0],1,(255,255,255),0)
TEXT_TITLE=Button([0,25],'WELCOME   TO   SWITCH',[0,0],0,(255,255,255),0)
BUTTON_BACK=Button([0,350],'BACK',[0,0],1,(255,255,255),0)
class Game:
    def __init__(self,WIDTH,HEIGHT,CLOCK,FPS,DISPLAY,COLOURS):
        self.WIDTH=WIDTH
        self.HEIGHT=HEIGHT
        self.CLOCK=CLOCK
        self.FPS=FPS
        self.DISPLAY=DISPLAY
        self.COLOURS=COLOURS
    def init(self):
        self.CLOCK=pygame.time.Clock()
        self.DISPLAY=pygame.display.set_mode((self.WIDTH,self.HEIGHT))
    def main_menu(self):
        BUTTON_LIST[:]=[]
        BUTTON_LIST.append(BUTTON_PLAY)
        BUTTON_LIST.append(BUTTON_QUIT)
        BUTTON_LIST.append(BUTTON_OPTIONS)
        BUTTON_LIST.append(TEXT_TITLE)
        while True:
            self.DISPLAY.fill((self.COLOURS[BLACK]))
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                    sys.exit()
                if event.type == pygame.MOUSEBUTTONDOWN:
                    if event.button == 1:
                        global LEFT_MOUSE_BUTTON_CLICKED
                        LEFT_MOUSE_BUTTON_CLICKED=1
                else:
                    LEFT_MOUSE_BUTTON_CLICKED=0
            for button in BUTTON_LIST:
                button.draw()
                button.detect_collision()
            pygame.display.update()
            clock.tick(24)
    def main(self):
        pass
    def options(self):
        BUTTON_LIST[:]=[]
        BUTTON_LIST.append(BUTTON_BACK)
        while True:
            self.DISPLAY.fill((self.COLOURS[BLACK]))
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                    sys.exit()
            for button in BUTTON_LIST:
                button.draw()
                button.detect_collision()
            pygame.display.update()
            clock.tick(24)
GAME=Game(DEFAULT_WIDTH,DEFAULT_HEIGHT,0,24,0,[(255,255,255),(0,0,0),(25,255,25),(255,25,25),(25,25,255),(125,125,125)])
GAME.init()
GAME.main_menu()
