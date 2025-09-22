import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *

# Diamond vertices (top, bottom, and square middle)
vertices = [
    [0, 1, 0],    # 0 - Top
    [0, -1, 0],   # 1 - Bottom
    [1, 0, 1],    # 2 - Front-right
    [-1, 0, 1],   # 3 - Front-left
    [-1, 0, -1],  # 4 - Back-left
    [1, 0, -1]    # 5 - Back-right
]

# Diamond faces (triangles)
triangles = [
    [0, 2, 3],  # Top front
    [0, 3, 4],  # Top left
    [0, 4, 5],  # Top back
    [0, 5, 2],  # Top right

    [1, 3, 2],  # Bottom front
    [1, 4, 3],  # Bottom left
    [1, 5, 4],  # Bottom back
    [1, 2, 5]   # Bottom right
]

# Colors for each face
colors = [
    (1, 0, 0),   # Red
    (0, 1, 0),   # Green
    (0, 0, 1),   # Blue
    (1, 1, 0),   # Yellow
    (1, 0, 1),   # Magenta
    (0, 1, 1),   # Cyan
    (1, 0.5, 0), # Orange
    (0.5, 0, 0.5) # Purple
]

def draw_diamond():
    glBegin(GL_TRIANGLES)
    for i, tri in enumerate(triangles):
        glColor3fv(colors[i % len(colors)])  # assign color per face
        for vertex in tri:
            glVertex3fv(vertices[vertex])
    glEnd()

def main():
    pygame.init()
    display = (800, 600)
    pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
    pygame.display.set_caption("3D Diamond - Lab Exercise")

    glEnable(GL_DEPTH_TEST)  # enable depth testing
    gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
    glTranslatef(0.0, 0.0, -7)   # move diamond backwards
    glScalef(0.7, 0.7, 0.7)      # scale down the diamond

    clock = pygame.time.Clock()
    running = True

    # Track keys held down
    keys = {
        "left": False,
        "right": False,
        "rot_up": False,
        "rot_down": False,
        "rot_left": False,
        "rot_right": False,
        "scale_in": False,
        "scale_out": False
    }

    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_a: keys["left"] = True
                if event.key == pygame.K_d: keys["right"] = True
                if event.key == pygame.K_w: keys["rot_up"] = True
                if event.key == pygame.K_s: keys["rot_down"] = True
                if event.key == pygame.K_q: keys["rot_left"] = True
                if event.key == pygame.K_e: keys["rot_right"] = True
                if event.key == pygame.K_z: keys["scale_in"] = True
                if event.key == pygame.K_x: keys["scale_out"] = True

            if event.type == pygame.KEYUP:
                if event.key == pygame.K_a: keys["left"] = False
                if event.key == pygame.K_d: keys["right"] = False
                if event.key == pygame.K_w: keys["rot_up"] = False
                if event.key == pygame.K_s: keys["rot_down"] = False
                if event.key == pygame.K_q: keys["rot_left"] = False
                if event.key == pygame.K_e: keys["rot_right"] = False
                if event.key == pygame.K_z: keys["scale_in"] = False
                if event.key == pygame.K_x: keys["scale_out"] = False

        # Apply transformations smoothly
        if keys["left"]: glTranslatef(-0.05, 0, 0)
        if keys["right"]: glTranslatef(0.05, 0, 0)
        if keys["rot_up"]: glRotatef(2, 1, 0, 0)
        if keys["rot_down"]: glRotatef(-2, 1, 0, 0)
        if keys["rot_left"]: glRotatef(2, 0, 1, 0)
        if keys["rot_right"]: glRotatef(-2, 0, 1, 0)
        if keys["scale_in"]: glScalef(0.99, 0.99, 0.99)
        if keys["scale_out"]: glScalef(1.01, 1.01, 1.01)

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        draw_diamond()
        pygame.display.flip()
        clock.tick(60)

    pygame.quit()

if __name__ == "__main__":
    main()
