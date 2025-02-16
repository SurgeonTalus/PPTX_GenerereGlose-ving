Advanced Presentation Generator with Draw Things API and LM Studio Integration

## Lange tekster fungerer ikke.  
## NY LØSNING Bruk Grpc for å unngå Json error. 

## Overview
This project is a Python-based tool designed to automate the creation of PowerPoint presentations. It utilizes the Draw Things API for generating custom images and LM Studio for advanced text completion. The tool processes input from a specified folder (`/Users/sondre/Downloads/gloser`), generates images and text, removes image backgrounds, and arranges the content into a beautifully formatted PowerPoint presentation.

## Future Plans. 
Upload own images to powerpoint. Make AI generate text and presentation based on those images. 

An example of a generated slide using SDXL3.5 Turbo 8bit on M1 MacBook Pro 16GB Ram
"![Your Image](https://raw.githubusercontent.com/SurgeonTalus/PPTX_GenerereGlose-ving/main/Gloseo%CC%88ving.png)"

## Features
- **Image Generation**: Integrates with Draw Things API to create images based on slide content.
- **Background Removal**: Utilizes the `dis_bg_remover` module to eliminate image backgrounds.
- **Text Processing**: Communicates with LM Studio for advanced text completion.
- **Dynamic Slide Layouts**: Automatically adjusts slide titles, subtitles, and image placements.
- **Custom Design Elements**: Includes background color calculations and semi-oval blur effects for polished visuals.

## Setup and Installation
### Prerequisites
Ensure the following dependencies are installed:
```sh
pip install requests json python-pptx Pillow numpy dis-bg-remover onnxruntime transformers opencv-python
```
Make sure `Draw Things.app` is installed in the `/Applications` directory and the API server is started from within the app.

### Directory Structure
The script processes files from:
```
/Users/sondre/Downloads/gloser
```
Ensure this directory exists and contains the input text files for slide generation.

## Configuration
### Constants
- `REMOVE_BACKGROUND = True`: Toggles background removal for generated images.
- `draw_things_app_path = "/Applications/Draw Things.app"`: Path to the Draw Things application.

## Launching Draw Things App
The script attempts to open Draw Things using:
```python
subprocess.Popen(["open", draw_things_app_path])
```
If the application fails to open, ensure the path is correct and the app is installed.

## Draw Things API Communication
### Checking Loaded Models
Before generating images, the script checks for a loaded model:
```python
models_response = requests.get("http://localhost:1234/api/v0/models")
```
It retrieves the model ID and uses it for image generation.

### Generating Images
Images are generated with the following settings:
- `IMG_SIZE = 512`
- `prompt`: Slide title and subtitle are combined as the image prompt.
- `negative_prompt`: Applied to enhance image quality by excluding unwanted features.

Example API call:
```python
response = requests.post(DRAW_THINGS_URL, json=params, headers=headers)
```
The images are saved temporarily and processed for background removal.

## Background Removal
The project utilizes the `dis_bg_remover` module with an ONNX model:
```python
model_path = "/Users/sondre/Downloads/isnet_dis.onnx"
```
### Troubleshooting
- Ensure the model file is present at the specified path.
- If errors occur, download the model again or check the ONNX runtime installation.

## Slide Design and Layout
### Dynamic Titles and Subtitles
Titles are extracted from lines beginning with `#`, `##`, or `###`. The remaining lines are treated as subtitles.

### Image Placement and Background Color
Images are added to slides in three layers:
1. **Background Image**: Blurred or original image.
2. **Main Image**: Processed image with background removed.
3. **Overlay**: Optional semi-oval blur effect for aesthetic enhancement.

### Custom Color Scheme
The average color of images is calculated to set the slide background:
```python
avg_color = calculate_average_color(image_path)
```
The color is brightened to ensure text readability.

## LM Studio Integration
The script communicates with LM Studio for text completion using:
```python
requests.post("http://localhost:1234/v1/chat/completions", json=request_data, stream=True)
```
- The `model` parameter is dynamically set based on the currently loaded model.
- `temperature` and `max_tokens` control text generation creativity and length.

## Error Handling and Debugging
- If the Draw Things API fails, a detailed error message is printed.
- Debug statements are included throughout the script to trace execution flow.
- Images are saved in the `~/Downloads` directory for easy review.

## Example Usage
```sh
python3 presentation_generator.py
```
Ensure `Draw Things.app` is running and the API server is active.

## Conclusion
This project automates the creation of visually appealing PowerPoint presentations by combining advanced AI image generation, text completion, and dynamic slide design. It is ideal for users looking to streamline content creation with custom visuals and well-structured layouts.

