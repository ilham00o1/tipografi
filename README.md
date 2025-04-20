#!/usr/bin/env python
# -*- coding: utf-8 -*-

# Script untuk membuat glyph Brontosaurus untuk FontLabs
# Pastikan FontForge sudah terinstal

import fontforge
import psMat
import math

def create_brontosaurus_font():
    # Membuat font baru
    font = fontforge.font()
    font.familyname = "BrontoFont"
    font.fullname = "BrontoFont Regular"
    font.fontname = "BrontoFont-Regular"
    font.copyright = "Created with FontForge for FontLabs"
    
    # Membuat glyph untuk karakter 'B' (Brontosaurus)
    glyph = font.createChar(66, "B")  # ASCII 66 adalah 'B'
    glyph.width = 800
    
    # Memulai editing pada glyph
    glyph.unlinkRef()
    glyph.clear()
    
    # -- Membuat bentuk Brontosaurus --
    
    # Badan utama (oval)
    pen = glyph.glyphPen()
    pen.moveTo(200, 300)
    pen.curveTo(200, 200, 500, 200, 500, 300)
    pen.curveTo(600, 300, 600, 500, 500, 500)
    pen.curveTo(500, 600, 200, 600, 200, 500)
    pen.curveTo(100, 500, 100, 300, 200, 300)
    pen.closePath()
    
    # Leher dan kepala
    pen.moveTo(500, 400)
    pen.curveTo(550, 450, 600, 550, 620, 650)
    pen.curveTo(630, 700, 650, 720, 680, 730)
    pen.curveTo(700, 735, 720, 730, 700, 720)  # Kepala
    pen.curveTo(730, 710, 730, 690, 720, 670)
    pen.curveTo(710, 650, 690, 620, 680, 600)
    pen.curveTo(660, 550, 630, 500, 600, 450)
    pen.curveTo(570, 400, 530, 380, 500, 400)
    pen.closePath()
    
    # Ekor
    pen.moveTo(200, 400)
    pen.curveTo(150, 420, 100, 450, 80, 500)
    pen.curveTo(50, 550, 30, 600, 20, 550)
    pen.curveTo(10, 500, 20, 450, 40, 400)
    pen.curveTo(70, 350, 120, 350, 200, 400)
    pen.closePath()
    
    # Kaki depan
    pen.moveTo(350, 250)
    pen.lineTo(330, 150)
    pen.lineTo(360, 150)
    pen.lineTo(380, 250)
    pen.closePath()
    
    # Kaki belakang
    pen.moveTo(250, 250)
    pen.lineTo(230, 150)
    pen.lineTo(260, 150)
    pen.lineTo(280, 250)
    pen.closePath()
    
    # Mata
    pen.moveTo(700, 720)
    pen.curveTo(705, 725, 710, 725, 710, 720)
    pen.curveTo(710, 715, 705, 715, 700, 720)
    pen.closePath()
    
    # -- Akhir bentuk Brontosaurus --
    
    # Generate font
    font.generate("BrontoFont.otf")
    print("Font berhasil dibuat: BrontoFont.otf")
    return font

def create_full_alphabet_with_bronto_theme():
    """
    Membuat seluruh alfabet dengan tema brontosaurus
    """
    font = fontforge.font()
    font.familyname = "BrontoAlphabet"
    font.fullname = "BrontoAlphabet Regular" 
    font.fontname = "BrontoAlphabet-Regular"
    font.copyright = "Created with FontForge for FontLabs"
    
    # Properti untuk semua huruf
    neck_angle = 0
    tail_length = 100
    body_size = 1.0
    
    # Membuat seluruh alfabet (A-Z dan a-z)
    for i in range(65, 91):  # A-Z
        create_bronto_char(font, i, neck_angle, tail_length, body_size)
        neck_angle += 5
        tail_length = (tail_length + 10) % 150
        body_size = 1.0 + (i - 65) * 0.01
    
    for i in range(97, 123):  # a-z
        create_bronto_char(font, i, neck_angle, tail_length, body_size * 0.8)
        neck_angle += 5
        tail_length = (tail_length + 10) % 150
        body_size = 1.0 + (i - 97) * 0.01
    
    # Generate font
    font.generate("BrontoAlphabet.otf")
    print("Font berhasil dibuat: BrontoAlphabet.otf")
    return font

def create_bronto_char(font, char_code, neck_angle, tail_length, body_size):
    """
    Membuat karakter dengan bentuk brontosaurus yang dimodifikasi
    """
    glyph = font.createChar(char_code)
    glyph.width = 800
    
    glyph.unlinkRef()
    glyph.clear()
    
    pen = glyph.glyphPen()
    
    # Skala dan parameter untuk modifikasi
    scale = body_size
    angle_rad = math.radians(neck_angle)
    
    # Badan utama (oval)
    pen.moveTo(200 * scale, 300 * scale)
    pen.curveTo(200 * scale, 200 * scale, 500 * scale, 200 * scale, 500 * scale, 300 * scale)
    pen.curveTo(600 * scale, 300 * scale, 600 * scale, 500 * scale, 500 * scale, 500 * scale)
    pen.curveTo(500 * scale, 600 * scale, 200 * scale, 600 * scale, 200 * scale, 500 * scale)
    pen.curveTo(100 * scale, 500 * scale, 100 * scale, 300 * scale, 200 * scale, 300 * scale)
    pen.closePath()
    
    # Leher dan kepala dengan rotasi
    base_x, base_y = 500 * scale, 400 * scale
    
    # Leher
    neck_points = [
        (500, 400),
        (550, 450), (600, 550), (620, 650),
        (630, 700), (650, 720), (680, 730)
    ]
    
    # Kepala
    head_points = [
        (680, 730),
        (700, 735), (720, 730), (700, 720),
        (730, 710), (730, 690), (720, 670),
        (710, 650), (690, 620), (680, 600),
        (660, 550), (630, 500), (600, 450),
        (570, 400), (530, 380), (500, 400)
    ]
    
    # Aplikasikan rotasi pada leher dan kepala
    neck_rotated = []
    for x, y in neck_points:
        x_scaled, y_scaled = x * scale, y * scale
        x_rot = base_x + (x_scaled - base_x) * math.cos(angle_rad) - (y_scaled - base_y) * math.sin(angle_rad)
        y_rot = base_y + (x_scaled - base_x) * math.sin(angle_rad) + (y_scaled - base_y) * math.cos(angle_rad)
        neck_rotated.append((x_rot, y_rot))
    
    head_rotated = []
    for x, y in head_points:
        x_scaled, y_scaled = x * scale, y * scale
        x_rot = base_x + (x_scaled - base_x) * math.cos(angle_rad) - (y_scaled - base_y) * math.sin(angle_rad)
        y_rot = base_y + (x_scaled - base_x) * math.sin(angle_rad) + (y_scaled - base_y) * math.cos(angle_rad)
        head_rotated.append((x_rot, y_rot))
    
    # Gambar leher
    pen.moveTo(neck_rotated[0][0], neck_rotated[0][1])
    for i in range(1, len(neck_rotated), 2):
        if i+1 < len(neck_rotated):
            pen.curveTo(neck_rotated[i][0], neck_rotated[i][1], 
                       neck_rotated[i+1][0], neck_rotated[i+1][1], 
                       neck_rotated[min(i+2, len(neck_rotated)-1)][0], neck_rotated[min(i+2, len(neck_rotated)-1)][1])
    
    # Gambar kepala
    for i in range(0, len(head_rotated), 2):
        if i+1 < len(head_rotated):
            pen.curveTo(head_rotated[i][0], head_rotated[i][1], 
                       head_rotated[i+1][0], head_rotated[i+1][1], 
                       head_rotated[min(i+2, len(head_rotated)-1)][0], head_rotated[min(i+2, len(head_rotated)-1)][1])
    
    pen.closePath()
    
    # Ekor dengan panjang yang bervariasi
    pen.moveTo(200 * scale, 400 * scale)
    pen.curveTo(150 * scale, 420 * scale, 100 * scale, 450 * scale, 80 * scale, 500 * scale)
    pen.curveTo(50 * scale, 550 * scale, 
               30 * scale, (600 - tail_length) * scale, 
               20 * scale, (550 - tail_length) * scale)
    pen.curveTo(10 * scale, 500 * scale, 20 * scale, 450 * scale, 40 * scale, 400 * scale)
    pen.curveTo(70 * scale, 350 * scale, 120 * scale, 350 * scale, 200 * scale, 400 * scale)
    pen.closePath()
    
    # Kaki depan
    pen.moveTo(350 * scale, 250 * scale)
    pen.lineTo(330 * scale, 150 * scale)
    pen.lineTo(360 * scale, 150 * scale)
    pen.lineTo(380 * scale, 250 * scale)
    pen.closePath()
    
    # Kaki belakang
    pen.moveTo(250 * scale, 250 * scale)
    pen.lineTo(230 * scale, 150 * scale)
    pen.lineTo(260 * scale, 150 * scale)
    pen.lineTo(280 * scale, 250 * scale)
    pen.closePath()
    
    # Mata
    eye_x = head_rotated[6][0]
    eye_y = head_rotated[6][1]
    pen.moveTo(eye_x, eye_y)
    pen.curveTo(eye_x + 5, eye_y + 5, eye_x + 10, eye_y + 5, eye_x + 10, eye_y)
    pen.curveTo(eye_x + 10, eye_y - 5, eye_x + 5, eye_y - 5, eye_x, eye_y)
    pen.closePath()

if __name__ == "__main__":
    # Membuat hanya satu karakter Brontosaurus (untuk karakter 'B')
    # create_brontosaurus_font()
    
    # Atau membuat seluruh alfabet dengan tema brontosaurus
    create_full_alphabet_with_bronto_theme()
