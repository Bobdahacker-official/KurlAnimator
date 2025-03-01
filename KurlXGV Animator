import pygame
import math
import random
import tkinter as tk
from tkinter import filedialog


pygame.init()


WIDTH, HEIGHT = 900, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("🔥 3D Music Visualizer - XGVanimations 🔥")


title_font = pygame.font.SysFont("Arial", 50, bold=True)
button_font = pygame.font.SysFont("Arial", 25)


pygame.mixer.init()

button_width, button_height = 250, 60
select_music_button = pygame.Rect(WIDTH // 2 - 125, HEIGHT // 2 - 50, button_width, button_height)
theme_toggle_button = pygame.Rect(WIDTH // 2 - 125, HEIGHT // 2 + 20, button_width, button_height)
volume_slider = pygame.Rect(WIDTH // 2 - 125, HEIGHT // 2 + 100, 250, 10)


visualizer_modes = ["Hyperspace Tunnel", "Particles", "Waves", "Stars"]
selected_visualizer = 0
visualizer_button = pygame.Rect(WIDTH // 2 - 125, HEIGHT // 2 + 160, button_width, button_height)


play_button = pygame.Rect(50, HEIGHT - 80, 150, 50)
pause_button = pygame.Rect(220, HEIGHT - 80, 150, 50)
stop_button = pygame.Rect(390, HEIGHT - 80, 150, 50)


is_playing = False
is_paused = False
animation_active = False
music_file = None
show_homepage = True
volume = 0.5
dark_mode = False

particles = []
def spawn_particles():
    global particles
    particles.clear()  # Clear previous particles
    for _ in range(100):
        particles.append({
            "x": random.uniform(-2, 2),
            "y": random.uniform(-2, 2),
            "z": random.uniform(2, 5),
            "vx": random.uniform(-0.1, 0.1),
            "vy": random.uniform(-0.1, 0.1),
            "vz": random.uniform(-0.2, -0.05),
            "size": random.randint(2, 5)
        })

def update_particles():
    for p in particles:
        p["x"] += p["vx"]
        p["y"] += p["vy"]
        p["z"] += p["vz"]
        if p["z"] < 0.1:
            p["z"] = random.uniform(2, 5)

def draw_particles():
    for p in particles:
        size = int(5 / (p["z"] + 3) * 8)
        pygame.draw.circle(screen, (255, 255, 255), project_3d(p["x"], p["y"], p["z"]), size)


def select_music():
    global music_file, show_homepage
    root = tk.Tk()
    root.withdraw()  # Hide tkinter window
    music_file = filedialog.askopenfilename(filetypes=[("MP3 Files", "*.mp3")])
    if music_file:
        pygame.mixer.music.load(music_file)
        pygame.mixer.music.set_volume(volume)
        show_homepage = False


def switch_visualizer():
    global selected_visualizer
    selected_visualizer = (selected_visualizer + 1) % len(visualizer_modes)


def project_3d(x, y, z, zoom=4):
    scale = 200 / (z + zoom)
    return int(WIDTH / 2 + x * scale), int(HEIGHT / 2 - y * scale)


def get_bg_color():
    return (20, 100, 20) if dark_mode else (180, 255, 180)


def draw_homepage():
    screen.fill(get_bg_color())


    time_elapsed = pygame.time.get_ticks() / 500
    green_shade = int((math.sin(time_elapsed) + 1) / 2 * 255)
    title_text = title_font.render("Welcome to XGVanimations", True, (green_shade, 255, green_shade))
    screen.blit(title_text, (WIDTH // 2 - title_text.get_width() // 2, 50))


    for rect, text in [
        (select_music_button, "Select Music File"),
        (theme_toggle_button, "Toggle Theme"),
        (visualizer_button, f"Visualizer: {visualizer_modes[selected_visualizer]}")
    ]:
        pygame.draw.rect(screen, (0, 150, 0), rect, border_radius=10)
        text_render = button_font.render(text, True, (255, 255, 255))
        screen.blit(text_render, (rect.x + 20, rect.y + 15))


    pygame.draw.rect(screen, (0, 100, 0), volume_slider)
    pygame.draw.circle(screen, (255, 255, 255), (volume_slider.x + int(volume * 250), volume_slider.y + 5), 8)

    volume_text = button_font.render(f"Volume: {int(volume * 100)}%", True, (0, 50, 0))
    screen.blit(volume_text, (WIDTH // 2 - 50, HEIGHT // 2 + 120))


def handle_homepage_buttons():
    global dark_mode, volume
    mouse_pos = pygame.mouse.get_pos()
    mouse_click = pygame.mouse.get_pressed()

    if select_music_button.collidepoint(mouse_pos) and mouse_click[0]:
        select_music()
    elif theme_toggle_button.collidepoint(mouse_pos) and mouse_click[0]:
        dark_mode = not dark_mode
    elif visualizer_button.collidepoint(mouse_pos) and mouse_click[0]:
        switch_visualizer()
    elif volume_slider.collidepoint(mouse_pos) and mouse_click[0]:
        volume = (mouse_pos[0] - volume_slider.x) / 250
        pygame.mixer.music.set_volume(volume)

def animate():
    if not animation_active:
        return
    screen.fill(get_bg_color())

    time_elapsed = pygame.time.get_ticks() / 1000
    scene = selected_visualizer

    if scene == 0:  # Hyperspace Tunnel
        for i in range(100):
            x, y, z = math.sin(i * 0.1 + time_elapsed) * 3, math.cos(i * 0.1 + time_elapsed) * 3, i * 0.1 + 3
            pygame.draw.circle(screen, (255, 255, 255), project_3d(x, y, z), 3)
    elif scene == 1:  # Particles
        draw_particles()
        update_particles()
    elif scene == 2:  # Waves
        for i in range(-5, 6):
            for j in range(-5, 6):
                x = i + math.sin(time_elapsed + i) * 0.5
                y = j + math.cos(time_elapsed + j) * 0.5
                pygame.draw.circle(screen, (255, 255, 255), project_3d(x, y, math.sin(time_elapsed * 3 + i + j) * 3), 3)


def draw_buttons():
    for rect, text, color in [
        (play_button, "Play", (0, 200, 0)),
        (pause_button, "Pause" if not is_paused else "Unpause", (200, 200, 0)),
        (stop_button, "Stop", (200, 0, 0))
    ]:
        pygame.draw.rect(screen, color, rect, border_radius=10)
        text_render = button_font.render(text, True, (0, 0, 0))
        screen.blit(text_render, (rect.x + 50, rect.y + 15))

def handle_buttons():
    global is_playing, is_paused, animation_active
    mouse_pos = pygame.mouse.get_pos()
    mouse_click = pygame.mouse.get_pressed()

    if play_button.collidepoint(mouse_pos) and mouse_click[0] and music_file:
        pygame.mixer.music.play(-1, 0.0)
        is_playing, animation_active, is_paused = True, True, False
    elif pause_button.collidepoint(mouse_pos) and mouse_click[0] and is_playing:
        is_paused = not is_paused
        pygame.mixer.music.pause() if is_paused else pygame.mixer.music.unpause()
        animation_active = not is_paused
    elif stop_button.collidepoint(mouse_pos) and mouse_click[0]:
        pygame.mixer.music.stop()
        is_playing, is_paused, animation_active = False, False, False


running = True
spawn_particles()

while running:
    screen.fill(get_bg_color())
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    if show_homepage:
        draw_homepage()
        handle_homepage_buttons()
    else:
        handle_buttons()
        animate()
        draw_buttons()

    pygame.display.flip()
    pygame.time.wait(10)

pygame.quit()




