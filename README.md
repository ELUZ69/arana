#pylint:disable=R1731

import random
import pygame
from pygame.locals import *
#import os
# define directorio de trabajo
#os.chdir('/storage/emulated/0/tareaPython')
try:
    f = open ('record.txt','r')
except:
    f = open('record.txt','w+')
    
RECf = f.read()

try:
    REC = int(RECf)
except:
    REC = 100000
tmrec = REC
f.close()

v = 5
rst = 30
cbal = 1
hi = hj = hk = hm = xp = cvel = contb = vu = 0
flg = con = disparo = recflag = col =  False
BL = (255,255,255)
RJ = (255,0,0)
AM = (255,250,80)
MO = (150,0,250)
VE = (0,255,0)
VO = (110,160,98)
NE = (0,0,0)
HH = (0,50,0)
AZ = (0,90,255)
fps = 40
ANCH = 1100
ALT = 900
PUNTOS = 100000
mper = (-v,-v)
#variable de movimiento permanente relativo de pelu
# inicia audio
pygame.mixer.init()
#pygame.mixer.__init__(init)
# Inicia pygame
pygame.init()
# inicia reloj en la varible clock
clock = pygame.time.Clock()
# inicia la pantalla con alto y ancho
screen = pygame.display.set_mode((ANCH, ALT))
#carga fondo
dir1 = pygame.image.load('fondo1.png').convert()
#crea nave
surf = pygame.image.load('nave.jpg').convert()
rect = surf.get_rect(center = (200,400))
surf.set_colorkey(NE)
o1 = pygame.image.load('nave1.jpg').convert()
o2 = pygame.image.load('nave2.jpg').convert()
o3 = pygame.image.load('nave3.jpg').convert()
o1.set_colorkey(NE)
o2.set_colorkey(NE)
o3.set_colorkey(NE)
lsurff = [o1,o2,o3]
#crea araña pelu
caraa = pygame.image.load('pelu.png').convert()
cara = pygame.transform.scale(caraa,(80,90))
mil = cara.get_rect(
                         center = (
                         random.randint(0,800),random.randint(0,700)
                         ) )
cara.set_colorkey(NE,RLEACCEL)
# transforma pelu
flp2 = pygame.transform.smoothscale(cara,(140,140))
flp1 = pygame.transform.flip(flp2,True,False)
milt = cara
#crea misiles
miSS = pygame.image.load('misil.png').convert()
mis = miSS.get_rect(center = (rect.midleft))
mis1 = miSS.get_rect( center = ( rect.midleft))
mis2 = miSS.get_rect ( center = (rect.midleft))
mis3 = miSS.get_rect ( center = (rect.midleft))
miSS.set_colorkey(BL)
miS = pygame.transform.flip(miSS,True,False)
bala = {
             1:[miS,mis,False] ,
             2:[miS,mis1,False],
             3:[miS,mis2,False],
             4:[miS,mis3,False],
             }
#texto: puntos, tiemps
hit = rst
tx = '%08d' %PUNTOS
trec = '%08d' %REC
rel = '%02d:%02d:%02d' %(hi,hj,hk)
bletr = 'DISPAROS: %04d' %contb
hits = 'HITS: %02d' % hit
Ltra = pygame.font.SysFont('Arial black',55,True)
hitx= Ltra.render(hits,False, HH)
rhit = hitx.get_rect( center = ((ANCH/2),ALT + 350))
texto = Ltra.render(tx,False,AM)
rtex = texto.get_rect(center = (825,25))
rec = Ltra.render(trec,False,RJ)
rrect = rec.get_rect (center = (480,25))
rrel = Ltra.render(rel,True,MO)
rectr = rrel.get_rect( center = (150,25))
sal = pygame.image.load('salir.png').convert()
salr = sal.get_rect(center = (ANCH-90,ALT+350))
salir = Ltra.render('SALIR',False,RJ)
rsal = salir.get_rect( center = (ANCH -100, ALT + 300))
sal.set_colorkey(BL)
rblet = Ltra.render(bletr,False,AZ)
blrec = rblet.get_rect( center = (200,ALT + 350))
#ajustes de audio
pygame.mixer.music.load("amb.mp3")
dispar = pygame.mixer.Sound("disparo.ogg")
prop = pygame.mixer.Sound("prop1.ogg")
aush = pygame.mixer.Sound('auch.ogg')
auch = pygame.mixer.Sound('rneg.ogg')
ja = pygame.mixer.Sound('risa.ogg')
sorry = pygame.mixer.Sound('risa_1.ogg')
pygame.mixer.music.set_volume (0.1)
pygame.mixer.music.play(loops = -1)
aush.set_volume(0.5)
dispar.set_volume(0.2)
prop.set_volume(0.2)
auch.set_volume(0.2)
ja.set_volume(0.1)
sorry.set_volume(1.0)
auk = [aush,auch]
#ciclo de juego
run = True
while run:
    pygame.display.update()
    #reloj, puntajes
    hm +=1
    if hm == 40:
        hk += 1
        hm = 0
    if hk == 60:
        hj += 1
        hk = 0
    if hj == 60:
        hi += 1
        hj = 0 
    if PUNTOS > REC and PUNTOS != 100000:
        flg = True
    if flg:
        REC = PUNTOS
#rendereo de letreros actualizados
    tx = '%08d' %PUNTOS
    texto = Ltra.render(tx,False,AM)
    trec = '%08d' %REC
    rec = Ltra.render(trec,False,RJ)
    rel = '%02d:%02d:%02d' %(hi ,hj ,hk)
    rrel = Ltra.render(rel,False,MO)
    rrel2 = Ltra.render(rel,False,VE)
    bletr = 'DISPAROS: %04d' %contb
    rblet = Ltra.render(bletr,False,AZ)
    hits = 'HITS: %02d' % hit
    hitx= Ltra.render(hits,False, HH)
    d = pygame.event.get()
    h,w = (rect.bottomright)
    screen.fill(VO)
# obtencion del ancho del rectangulo del fondo
    ra = dir1.get_rect().width
# rutina de despllazamiento del fondo
    xr = xp % ra
    screen.blit(dir1,(xr - ra,0))
    if xr -255 < ANCH:
        screen.blit(dir1,(xr,0))
    xp -= 6
    screen.blit(surf,rect)
# rutina de movimiento pelu y puntaje
    if mil.colliderect(rect) and PUNTOS != 100000 and PUNTOS != 0:
        screen.blit(lsurff[vu], rect)
        pygame.time.wait(5)
        vu += 1
        if vu == 3:
            vu = 0
        if  not col :
            ja.play()
            col = True
# pelu roba
    if not mil.colliderect(rect) and col:
        col = False
        sorry.play()
        PUNTOS -= random.randint(int(PUNTOS/2),PUNTOS)
        if PUNTOS  <= 0:
            PUNTOS = 0
 # movimiento pelu
    mil.move_ip(mper)
    if mil.top <= 0:
        mil.top = 0
        if mper == (-v,-v):
            mper = (-v,v)
        else:
            mper = (v,v)
    if mil.left <= 0:
        mil.left = 0
        if mper == (-v,-v):
            mper = (v,-v)
        else:
            mper = (v,v)
    if mil.right >= ANCH:
        mil.right = ANCH
        if mper == (v,v):
            mper = (-v,v)
        else:
            mper = (-v,-v)
    if mil.bottom >= ALT:
        mil.bottom = ALT
        if mper == (-v,v):
            mper = (-v,-v)
        else:
            mper = (v,-v) 
#control de ingreso de teclado:  movimiento nave, diaparo y exit
    for x in d:
        if x.type == KEYDOWN and x.key == K_0:
            run = False
        if x.type == KEYDOWN and x.key == K_SPACE:
            disparo = True
            bala[cbal][2] = True
            bala[cbal][1].update((h-48,w-17),(15,15))
            dispar.play()
            contb += 1
            cvel += 1
            if cbal == 4:
                cbal = 1
            else:
                cbal += 1
        if x.type == KEYDOWN and x.key == K_w:
            rect.move_ip(0,-30)
            if rect.top <= 0:
                rect.top = 0
            prop.play()
        if x.type == KEYDOWN and x.key == K_s:
            rect.move_ip(0,30)
            if rect.bottom >= ALT+100:
                rect.bottom = ALT+100
            prop.play()
        if x.type == KEYDOWN and x.key == K_a:
            rect.move_ip(-30,0)
            if rect.left <= 0:
                rect.left = 0
            prop.play()
        if x.type == KEYDOWN and x.key == K_d:
            rect.move_ip(30,0)
            if rect.right >= ANCH:
                rect.right = ANCH
            prop.play() 
#disparo movimiento y colicion de misiles
    if disparo:
        for i in  bala:
            if bala[i][2]:
                bala[i][1].move_ip(40,0)  
                screen.blit(bala[i][0],bala[i][1])
                if bala[i][1].left >= ANCH:
                    bala[i][2] = False
                if bala[i][1].colliderect(mil):
                   # suma puntos
                    PUNTOS += random.randint(1,(int(PUNTOS/2)+1))
                    for p in auk:
                        p.play()
                    bala [i][2] = False
                    if cvel >= 300:
                        hit -= 1
                    if  not con:
                         milt = flp1
                         con = True
                    else:
                         con = False
                         milt = cara
# control velocidae pelu
    if  cvel >= 100:
        v = 6
        if contb >=200:
            v = 7
#reloj, canbio de color
    if cvel >= 300:# or hj >= 3:
        rrelt = rrel2
        recflag = True
        v = 9
        if hit == 0:
            v = 5
            cvel = 0
            rst += 5
            hit = rst
    if contb <= 300 and not recflag:
        rrelt = rrel
    screen.blit(milt,mil)
    screen.blit(rec,rrect)
    screen.blit(sal,salr)
    screen.blit(salir,rsal)
    screen.blit(texto,rtex)
    screen.blit(hitx,rhit)
    screen.blit(rrelt,rectr)
    screen.blit(rblet,blrec)
    clock.tick(fps)
if PUNTOS > tmrec and recflag :
    f = open ( 'record.txt','w')
    f.write(str(REC))
    f.close()
dispar.stop()
prop.stop()
auch.stop()
pygame.mixer.quit()
pygame.quit()
