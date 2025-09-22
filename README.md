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

# Colors for each pair of triangles
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
        glColor3fv(colors[i // 2])  # same color for each face (2 triangles)
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

    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                return

            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_a:  # Move left
                    glTranslatef(-0.2, 0, 0)
                if event.key == pygame.K_d:  # Move right
                    glTranslatef(0.2, 0, 0)
                if event.key == pygame.K_w:  # Move up
                    glTranslatef(0, 0.2, 0)
                if event.key == pygame.K_s:  # Move down
                    glTranslatef(0, -0.2, 0)
                if event.key == pygame.K_q:  # Rotate left
                    glRotatef(10, 0, 1, 0)
                if event.key == pygame.K_e:  # Rotate right
                    glRotatef(-10, 0, 1, 0)
                if event.key == pygame.K_z:  # Scale smaller
                    glScalef(0.9, 0.9, 0.9)
                if event.key == pygame.K_x:  # Scale bigger
                    glScalef(1.1, 1.1, 1.1)

        glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT)
        draw_cube()
        pygame.display.flip()
        clock.tick(30)

if __name__ == "__main__":
    main()
