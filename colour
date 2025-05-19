import cv2
import numpy as np
import tkinter as tk
from tkinter import filedialog
from PIL import Image, ImageTk
from scipy.spatial import distance

# Sample dataset of color names and their RGB values (you can expand this)
COLOR_DATA = {
    "red": (255, 0, 0),
    "green": (0, 255, 0),
    "blue": (0, 0, 255),
    "yellow": (255, 255, 0),
    "cyan": (0, 255, 255),
    "magenta": (255, 0, 255),
    "white": (255, 255, 255),
    "black": (0, 0, 0),
    "light_gray": (200, 200, 200),
    "dark_gray": (100, 100, 100),
    "orange": (255, 165, 0),
    "purple": (128, 0, 128),
    "pink": (255, 192, 203),
    "brown": (139, 69, 19),
}

def closest_color(rgb):
    """Finds the closest color name in the dataset to the given RGB value."""
    min_distance = float('inf')
    closest_name = None
    for name, color_rgb in COLOR_DATA.items():
        dist = distance.euclidean(rgb, color_rgb)
        if dist < min_distance:
            min_distance = dist
            closest_name = name
    return closest_name, COLOR_DATA[closest_name]

class ColorDetectorGUI:
    def __init__(self, master):
        self.master = master
        master.title("Interactive Color Detector")

        self.image_path = tk.StringVar()

        # Image Selection
        self.browse_button = tk.Button(master, text="Browse Image", command=self.browse_image)
        self.browse_button.pack(pady=10)

        self.image_label = tk.Label(master, text="No image selected", width=400, height=300)
        self.image_label.pack(pady=5)
        self.image_label.bind("<Button-1>", self.on_image_click)
        self.pil_image = None  # To store the PIL Image object

        # Color Information Display
        self.color_info_frame = tk.LabelFrame(master, text="Detected Color")
        self.color_info_frame.pack(pady=10, padx=10)

        self.rgb_label = tk.Label(self.color_info_frame, text="RGB: ")
        self.rgb_label.pack()

        self.name_label = tk.Label(self.color_info_frame, text="Closest Name: ")
        self.name_label.pack()

        self.color_preview_canvas = tk.Canvas(self.color_info_frame, width=50, height=50, bg="white")
        self.color_preview_canvas.pack(pady=5)

    def browse_image(self):
        file_path = filedialog.askopenfilename(
            title="Select an Image File",
            filetypes=[("Image files", "*.png;*.jpg;*.jpeg;*.gif;*.bmp")]
        )
        if file_path:
            self.image_path.set(file_path)
            self.display_selected_image(file_path)

    def display_selected_image(self, path):
        try:
            self.pil_image = Image.open(path)
            resized_img = self.pil_image.resize((400, 300))
            self.img_tk = ImageTk.PhotoImage(resized_img)
            self.image_label.config(image=self.img_tk)
            self.image_label.image = self.img_tk
        except Exception as e:
            print(f"Error displaying image: {e}")
            self.image_label.config(image="")
            self.pil_image = None

    def on_image_click(self, event):
        if self.pil_image:
            x = event.x
            y = event.y
            try:
                rgb = self.pil_image.getpixel((x, y))
                if isinstance(rgb, int):  # Handle grayscale images
                    rgb = (rgb, rgb, rgb)
                elif len(rgb) == 4:  # Handle RGBA images, ignore alpha
                    rgb = rgb[:3]

                closest_name, closest_rgb = closest_color(rgb)

                self.rgb_label.config(text=f"Clicked RGB: ({rgb[0]}, {rgb[1]}, {rgb[2]})")
                self.name_label.config(text=f"Closest Name: {closest_name}")
                self.update_color_preview(closest_rgb)

            except IndexError:
                print("Clicked outside the image boundaries.")
            except Exception as e:
                print(f"Error getting pixel data: {e}")

    def update_color_preview(self, rgb):
        hex_color = "#{:02x}{:02x}{:02x}".format(rgb[0], rgb[1], rgb[2])
        self.color_preview_canvas.config(bg=hex_color)

if __name__ == "__main__":
    root = tk.Tk()
    gui = ColorDetectorGUI(root)
root.mainloop()
