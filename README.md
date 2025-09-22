import pygame
from pygame.locals import *
from OpenGL.GL import *
from OpenGL.GLU import *

# Cube vertices
vertices = [
    [1, 1, 1],    # 0
    [1, 1, -1],   # 1
    [1, -1, -1],  # 2
    [1, -1, 1],   # 3
    [-1, 1, 1],   # 4
    [-1, -1, -1], # 5
    [-1, -1, 1],  # 6
    [-1, 1, -1]   # 7
]

# Triangles (12 total)
triangles = [
    [0, 1, 2], [0, 2, 3],  # Right face
    [4, 6, 5], [4, 5, 7],  # Left face
    [0, 3, 6], [0, 6, 4],  # Front face
    [1, 7, 5], [1, 5, 2],  # Back face
    [0, 4, 7], [0, 7, 1],  # Top face
    [3, 2, 5], [3, 5, 6]   # Bottom face
]

# Colors for each face
colors = [
    (1, 0, 0),   # Red
    (0, 1, 0),   # Green
    (0, 0, 1),   # Blue
    (1, 1, 0),   # Yellow
    (1, 0, 1),   # Magenta
    (0, 1, 1)    # Cyan
]

def draw_cube():
    glBegin(GL_TRIANGLES)
    for i, tri in enumerate(triangles):
        glColor3fv(colors[i // 2])  # same color per face (2 triangles)
        for vertex in tri:
            glVertex3fv(vertices[vertex])
    glEnd()

def main():
    pygame.init()
    display = (800, 600)
    pygame.display.set_mode(display, DOUBLEBUF | OPENGL)
    pygame.display.set_caption("04 Lab 1")

    glEnable(GL_DEPTH_TEST)  # enable depth testing
    gluPerspective(45, (display[0] / display[1]), 0.1, 50.0)
    glTranslatef(0.0, 0.0, -7)   # move cube backwards
    glScalef(0.7, 0.7, 0.7)      # scale down the cube

    clock = pygame.time.Clock()
    running = True

    # Track keys held down
    keys = {
        "left": False,
        "right": False,
        "up": False,
        "down": False,
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
                if event.key == pygame.K_w: keys["up"] = True
                if event.key == pygame.K_s: keys["down"] = True
                if event.key == pygame.K_q: keys["rot_left"] = True
                if event.key == pygame.K_e: keys["rot_right"] = True
                if event.key == pygame.K_z: keys["scale_in"] = True
                if event.key == pygame.K_x: keys["scale_out"] = True

            if event.type == pygame.KEYUP:
                if event.key == pygame.K_a: keys["left"] = False
                if event.key == pygame.K_d: keys["right"] = False
                if event.key == pygame.K_w: keys["up"] = False
                if event.key == pygame.K_s: keys["down"] = False
                if event.key == pygame.K_q: keys["rot_left"] = False
                if event.key == pygame.K_e: keys["rot_right"] = False
                if event.key == pygame.K_z: keys["scale_in"] = False
                if event.key == pygame.K_x: keys["scale_out"] = False

        # Apply transformations smoothly
        if keys["left"]: glTranslatef(-0.05, 0, 0)
        if keys["right"]: glTranslatef(0.05, 0, 0)
        if keys["up"]: glTranslatef(0, 0.05, 0)
        if keys["down"]: glTranslatef(0, -0.05, 0)
        if keys["rot_left"]: glRotatef(2, 0, 1, 0)
        if keys["rot_right"]: glRotatef(-2, 0, 1, 0)
        if keys["scale_in"]: glScalef(0.99, 0.99, 0.99)
        if keys["scale_out"]: glScalef(1.01, 1.01, 1.01)

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        draw_cube()
        pygame.display.flip()
        clock.tick(60)  # smooth 60 FPS

    pygame.quit()

if __name__ == "__main__":
    main()
