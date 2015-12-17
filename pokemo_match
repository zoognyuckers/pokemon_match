import pygame, sys, os, random
from pygame.locals import *

BLACK = (  0,  0,  0)
WHITE = (255,255,255)
DGRAY = ( 55, 55, 55)
LGRAY = (200,200,200)
FPS = 60
clock = pygame.time.Clock()
rootImg = 'c:/_/sprites/anime/200x200/150/'
pygame.init()
displaysurf = pygame.display.set_mode((700,500),0,32)

resetBox = pygame.Rect((0,0,0,0))
numClick = 0
clickCounterBox = pygame.Rect((0,0,0,0))

class Image:
    speed = 5
    def __init__(self, path, loc, size):
        name = path[path.index('150/')+4:path.index('.jpg')].split(' ')
        self.id = name.pop(0)
        self.name = ' '.join(name)
        self.size = size
        self.coverWidth = self.size
        self.img = pygame.transform.scale(pygame.image.load(path),(self.size,self.size))
        self.rect = pygame.Rect((loc[0],loc[1],self.size,self.size))
        self.speed = 5
        textObj = pygame.font.Font('freesansbold.ttf', 15)
        self.nameText = textObj.render(self.name, True, BLACK, WHITE)
        self.nameRect = self.nameText.get_rect()
        self.nameRect.center = self.rect.center
        self.nameRect.bottom = self.rect.bottom

        self.numText = textObj.render(self.id, True, BLACK, WHITE)
        self.numRect = self.numText.get_rect()
        self.numRect.top = self.rect.top + 2
        self.numRect.left = self.rect.left + 2


    def gray(self):
        x, y, w, h = self.rect
        pygame.draw.rect(displaysurf, (DGRAY), (x, y, self.coverWidth, h))
    def blit(self):
        displaysurf.blit(self.img, self.rect)
        displaysurf.blit(self.nameText, self.nameRect)
        displaysurf.blit(self.numText, self.numRect)
        self.coverWidth += self.speed
        if self.coverWidth > self.size: self.coverWidth = self.size
        if self.coverWidth < 0: self.coverWidth = 0
        self.gray()
    def isClicked(self, pos):
        return self.rect.collidepoint(pos)
    def cover(self, cover = True):
        if cover: self.speed = 5
        else: self.speed = -5

def text_display():
    global resetBox, numClick, clickCounterBox
    fontObj = pygame.font.Font('freesansbold.ttf',32)

    resetButton = fontObj.render('reset', True, BLACK, LGRAY)
    resetBox = resetButton.get_rect()
    resetBox.right = 685
    resetBox.bottom = 485

    clickText = fontObj.render('clicks: ' + str(numClick), True, BLACK, LGRAY)
    clickCounterBox = clickText.get_rect()
    clickCounterBox.left = 540
    clickCounterBox.top = 40
    displaysurf.blit(clickText, clickCounterBox)
    displaysurf.blit(resetButton, resetBox)
def reset_Board():
    global numClick
    numClick = 0
    GAMESIZE = 500
    BOARDER = 10
    GAP = 5
    numBoxes = 4 # per row/collom
    boxSize = int(((GAMESIZE - BOARDER*2) / numBoxes) - GAP)
    imagePaths = [rootImg + i for i in os.listdir(rootImg)]
    random.shuffle(imagePaths)
    locations = []
    images = []
    for x in range(numBoxes):
        for y in range(numBoxes):
            locations.append((BOARDER + (GAP + boxSize) * x, BOARDER + (GAP + boxSize) * y))
    random.shuffle(locations)
    for loc in locations:
        images.append(Image(imagePaths[locations.index(loc)//2], loc, boxSize))

    return images
def update(images):
    displaysurf.fill(BLACK)
    text_display()
    stillCovering = True
    while stillCovering:
        stillCovering = False
        for img in images:
            img.blit()
            if img.coverWidth not in (0, img.size):
                stillCovering = True
        pygame.display.update()
        clock.tick(FPS)
def getClick(event):
    global click, numClick
    if event.type == MOUSEBUTTONUP:
        for img in images:
            if img.isClicked(event.pos) and img.coverWidth:
                numClick += 1
                click.append(img)
                img.cover(False)
                update(images)
                if len(click) >= 2:
                    if click[0].id != click[1].id:
                        pygame.time.wait(500)
                        click[0].cover()
                        click[1].cover()
                    click = []
        if resetBox.collidepoint(event.pos):
            return reset_game()
        pygame.event.clear()
    return None
def preview(images):
    for img in images:
        img.cover(False)
    update(images)
    pygame.time.wait(2000)
    for img in images:
        img.cover()
    update(images)
    pygame.event.clear()
def isWon():
    won = True
    for img in images:
        if img.coverWidth:
            won = False
    if won:
        print('you won')
    return won
def quit_event(event):
    if event.type == QUIT:
        pygame.quit()
        sys.exit()
def reset_game():
    images = reset_Board()
    preview(images)
    return images



images = reset_game()
click = []
while True:
    for event in pygame.event.get():
        quit_event(event)
        doRest = getClick(event)
        if doRest:
            images = doRest
    update(images)
    won = isWon()
    if won:
        images = reset_game()
        preview(images)


