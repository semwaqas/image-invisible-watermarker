```markdown
# Image Watermarker

[![PyPI version](https://badge.fury.io/py/image-watermarker.svg)](https://badge.fury.io/py/image-watermarker)

A Python package for embedding and extracting watermarks in various image formats (PNG, JPEG, WebP, AVIF, HEIC/HEIF).  It provides a simple and flexible way to protect your images by adding hidden or invisible watermarks.

## Features

*   **Multiple Image Formats:** Supports PNG, JPEG, WebP, AVIF, and HEIC/HEIF image formats.
*   **Different Watermarking Techniques:**
    *   **PNG & WebP:** Embeds the watermark text into the least significant bit (LSB) of the alpha channel.  This is a steganographic technique, making the watermark invisible to the naked eye.
    *   **JPEG:** Embeds the watermark text into the EXIF metadata (Artist field).  This method doesn't alter the visual appearance of the image.
    *   **AVIF & HEIC/HEIF:**  Appends a custom watermark marker and the watermark text to the end of the file bytes. This method is simpler but may be more easily detectable.
*   **Easy-to-Use API:**  Simple functions for adding and checking watermarks.
*   **Handles Bytes and File Paths:** Functions accept image data as bytes, `PIL.Image` objects, or file paths.
*   **Error Handling:** Includes error handling and informative error messages.
*   **Dependencies handled:** Minimal external dependencies for maximum compatibility.

## Installation

```bash
pip install image-watermarker
```

## Usage

The package provides separate functions for each supported image format:

*   `add_watermark_png(image, watermark_text)` and `check_watermark_png(image, watermark_text)`
*   `add_watermark_jpeg(image_bytes, watermark_text)` and `check_watermark_jpeg(image_bytes, watermark_text)`
*   `add_watermark_webp(image, watermark_text, output_path=None)` and `check_watermark_webp(image, watermark_text)`
*   `add_watermark_avif_heic_heif(file_bytes, watermark_text)` and `extract_watermark_avif_heic_heif(file_bytes, watermark_text)`

### PNG Example

```python
from image_watermarker import add_watermark_png, check_watermark_png

# Add watermark to a PNG image
image_bytes = add_watermark_png("input.png", "My Watermark")
if image_bytes:
    with open("output.png", "wb") as f:
        f.write(image_bytes)

# Check for the watermark
result = check_watermark_png("output.png", "My Watermark")
print("IsDetected:", result[0])  # Output: IsDetected: True
print("Detected Text:", result[1])  # Output: Detected Text: My Watermark

# You can also pass image bytes directly:
with open("input.png", "rb") as f:
    image_bytes = f.read()
watermarked_bytes = add_watermark_png(image_bytes, "Another Watermark")
# ...
```

### JPEG Example

```python
from image_watermarker import add_watermark_jpeg, check_watermark_jpeg

# Add watermark to a JPEG image
image_bytes = add_watermark_jpeg("input.jpg", "JPEG Watermark")
if image_bytes:
    with open("output.jpg", "wb") as f:
        f.write(image_bytes)

# Check for the watermark
result = check_watermark_jpeg("output.jpg", "JPEG Watermark")
print("IsDetected:", result[0])
print("Detected Text:", result[1])

# Using file paths:
image_bytes = add_watermark_jpeg("input.jpg", "Another JPEG Watermark") # Pass file path directly
```

### WebP Example

```python
from image_watermarker import add_watermark_webp, check_watermark_webp

# Add watermark to a WebP image
image_bytes = add_watermark_webp("input.webp", "WebP Watermark")
if image_bytes:
    with open("output.webp", "wb") as f:
        f.write(image_bytes)

# Check for the watermark
result = check_watermark_webp("output.webp", "WebP Watermark")
print("IsDetected:", result[0])
print("Detected Text:", result[1])
```

### AVIF/HEIC/HEIF Example

```python
from image_watermarker import add_watermark_avif_heic_heif, extract_watermark_avif_heic_heif

# Add watermark
image_bytes = add_watermark_avif_heic_heif("input.avif", "AVIF Watermark") # or .heic, .heif
if image_bytes:
    with open("output.avif", "wb") as f:
        f.write(image_bytes)

# Check watermark
result = extract_watermark_avif_heic_heif("output.avif", "AVIF Watermark")
print("IsDetected:", result[0])
print("Detected Text:", result[1])

# Example with file path:
image_bytes = add_watermark_avif_heic_heif("input.heic", "HEIC Watermark")
```

## Important Notes

*   **Alpha Channel (PNG & WebP):** The PNG and WebP watermarking methods require images to have an alpha channel.  If your image doesn't have one, it will be converted to RGBA.
*   **LSB Watermarking (PNG & WebP):**  LSB watermarking is generally robust to minor image manipulations (e.g., resizing, slight color adjustments). However, significant changes to the alpha channel or lossy compression can remove or corrupt the watermark.
*   **EXIF Metadata (JPEG):**  Watermarks stored in EXIF metadata can be easily removed by stripping the metadata from the image. This method is less secure than LSB watermarking.  Consider it more of a visible attribution than a robust protection mechanism.
* **AVIF and HEIC:**
    * AVIF and HEIC/HEIF watermarking relies on appending data.  This is *not* a robust watermarking technique. It's easily removed. It's mainly intended for simple cases where a basic, easily detectable watermark is sufficient.
*   **Error Handling:** The functions return `None` or raise exceptions if errors occur.  Check the return values and handle potential exceptions appropriately.  Error messages are also printed to the console.
*   **Dependencies:** The package relies on `Pillow` (PIL), `numpy`, and `piexif`. These are automatically installed when you install `image-watermarker` via pip.

## Contributing

Contributions are welcome!  If you find a bug or have a feature request, please open an issue on the GitHub repository. If you'd like to contribute code, please fork the repository and submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. (You'll need to create a LICENSE file and put the MIT license text in it.)
```