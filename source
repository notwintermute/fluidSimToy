import math
import pygame
import numpy as np
import time

sqSide = 4
noSq = 100
sqRes = noSq * sqSide
resX = sqRes
resY = sqRes
n = noSq
grid = np.zeros((n,n))  # grid where amount is stored

# dividing matrix
aDiv = 9*np.ones((n, n))
line1 = 6*np.ones((1, n))
line1[0, 0] = line1[0, -1] = 4
aDiv[0,:] = aDiv[-1, :]= aDiv[:, 0] = aDiv[:, -1] = line1
# mult matrix
line2 = np.zeros((1,n))
line2[0, 0:2] = 1
aMul = line2
line3 = np.zeros((1,n+1))
line3[0, 0:3] = 1
for i in range(n-3):
    aMul = np.concatenate((aMul, line3), axis=1)
aMul = np.concatenate((aMul, np.ones((1,3))), axis=1)
aMul = np.concatenate((aMul, np.zeros((1,n))), axis=1)
aMul[0, -1] = aMul[0, -2] = 1
aMul = aMul.reshape((n, n))

emitters = []
brushSize = 0
drawing = False
erasing = False
pygame.init()

# initialize surface and start the main loop
surface = pygame.display.set_mode((resX, resY))
pygame.display.set_caption('fluidsim')
running = True
# --------------------------------------- Main Loop ---------------------------------------
while running:
    mouse = pygame.mouse.get_pos()  # puts the mouse position into a 2d tuple
    mouseSq = [math.floor(mouse[0] / sqSide), math.floor(mouse[1] / sqSide)]
    # ------------------------------------- input handling -------------------------------------
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
            break
        # ------------------------------------ mouse click actions ------------------------------------
        if event.type == pygame.MOUSEBUTTONDOWN:
            if event.button == 1:
                drawing = True
            if event.button == 3:
                erasing = True
        if event.type == pygame.MOUSEBUTTONUP:  # releasing the hold
            drawing = False
            erasing = False
        # ------------------------------------ key press actions ------------------------------------
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_s:
                grid = np.matmul(aMul, np.matmul(aMul, grid).transpose()) / aDiv
            if event.key == pygame.K_a:
                brushSize += 1
            if event.key == pygame.K_z:
                brushSize -= 1
            if event.key == pygame.K_q:
                emitters.append(mouseSq)

    # ---------------------------------------- Updating Parameters ----------------------------------------
    if drawing:
        for x in range(2*brushSize+1):
            for y in range(2*brushSize+1):
                grid[mouseSq[0] + x-brushSize, mouseSq[1]+ y-brushSize] = 255
    if erasing:
        for x in range(2 * brushSize + 1):
            for y in range(2 * brushSize + 1):
                grid[mouseSq[0] + x - brushSize, mouseSq[1] + y - brushSize] = 0

    for p in emitters:
        grid[p[0]][p[1]] = 100

    grid = (np.matmul(aMul, np.matmul(aMul, grid).transpose()) / aDiv).transpose()
    # ---------------------------------------- Rendering ----------------------------------------
    surface.fill((0, 0, 0))  # resets the screen to black
    for x in range(noSq):
        for y in range(noSq):
            color = (grid[x,y], grid[x,y], grid[x,y])
            pygame.draw.rect(surface, color, (x*sqSide, y*sqSide, sqSide, sqSide))
    pygame.display.flip()
