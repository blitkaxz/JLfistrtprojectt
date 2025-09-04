import pygame
import sys
import math

# Inisialisasi pygame
pygame.init()

# Ukuran layar
WIDTH, HEIGHT = 800, 800
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Jenny istri Leon 💖")

# Warna
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Font
font = pygame.font.SysFont("Arial", 28, bold=True)

# Pusat layar
CENTER_X, CENTER_Y = WIDTH // 1.5, HEIGHT // 1.5

# Variabel
angle = 0
texts = []  # simpan teks

# Load gambar Leon (pastikan ada file leon.png)
try:
    leon_img = pygame.image.load("leon.png")
    leon_img = pygame.transform.scale(leon_img, (200, 200))  # resize agar pas
except:
    leon_img = None  # kalau gagal load, biar gak error

# Rumus koordinat love parametris
def heart_path(t, scale=20):
    x = 16 * math.sin(t) ** 3
    y = 13 * math.cos(t) - 5 * math.cos(2*t) - 2 * math.cos(3*t) - math.cos(4*t)
    return int(CENTER_X + scale * x), int(CENTER_Y - scale * y)

# Loop utama
clock = pygame.time.Clock()
running = True
while running:
    screen.fill(BLACK)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        # klik mouse → tambah teks
        if event.type == pygame.MOUSEBUTTONDOWN:
            texts.append(len(texts))  # simpan index urut

    # Gambar semua teks dalam orbit love
    for i, idx in enumerate(texts):
        t = angle + i * (2 * math.pi / max(1, len(texts)))
        x, y = heart_path(t, scale=30)
        text = font.render("Jenny istri Leon", True, RED)
        rect = text.get_rect(center=(x, y))
        screen.blit(text, rect)
        
            # Jika sudah 20 klik → tampilkan gambar Leon
    if len(texts) >= 20 and leon_img:
        rect = leon_img.get_rect(center=(CENTER_X, CENTER_Y))
        screen.blit(leon_img, rect)

    # Update sudut
    angle += 0.02

    pygame.display.flip()
    clock.tick(60)

pygame.quit()
sys.exit()
