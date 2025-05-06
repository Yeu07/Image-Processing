# Procesamiento de Imágenes - 2025
## TP3

### Integrantes
- **Andrés Maglione** - **13753**
- **Yeumen Silva** - **13693**

### Introducción
El presente trabajo práctico corresponde a la unidad 2 de la materia Procesamiento de Imágenes. En esta ocasión, se abordan temas relacionados con histogramas, combinación de imágenes y técnicas aplicadas en el dominio espacial, utilizando Python junto con librerías como OpenCV, NumPy y Matplotlib.

El objetivo principal es profundizar en el análisis y manipulación de imágenes digitales a través del uso de histogramas, la fusión de diferentes imágenes y la aplicación de filtros espaciales, fortaleciendo así la comprensión de estos conceptos fundamentales del procesamiento de imágenes.

Este documento presenta una breve descripción de los ejercicios realizados y las respuestas a las preguntas teóricas incluidas en el trabajo práctico. Para consultar el desarrollo completo y los resultados obtenidos, se puede acceder al notebook con el código fuente (`TP3.ipynb`) o al PDF generado a partir del mismo (`TP3.pdf`).

### Ejercicios

#### 4. (*) Apertura y clausura morfológica: Aplicar apertura y clausura para eliminar ruido o cerrar huecos. Comparar la imagen original y la resultante de aplicar el operador. Comentar los efectos visuales. Comparar con los resultados anteriores. Mostrar 4 subplots: original, apertura, cierre, diferencia entre ambos.
![img1](./reporte_imagenes/tp3-1.png)

--- 
Fragmento de código:
```python
image_path = './img_binarias/bin2.png'
image = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)

kernelToUse = kernel4

# opening
opening = cv2.morphologyEx(image, cv2.MORPH_OPEN, kernelToUse)
# closing
closing = cv2.morphologyEx(image, cv2.MORPH_CLOSE, kernelToUse)

plt.figure(figsize=(16, 8))
# 1. Original Image
plt.subplot(1, 4, 1)
plt.imshow(image, cmap='gray')
plt.title('Imagen original')
plt.axis('off')

# 2. clausura
plt.subplot(1, 4, 2)
plt.imshow(closing, cmap='gray')
plt.title('Clausura')
plt.axis('off')

# 3. apertura
plt.subplot(1, 4, 3)
plt.imshow(opening, cmap='gray')
plt.title('Apertura')
plt.axis('off')


#4. apertura - clausura
plt.subplot(1, 4, 4)
plt.imshow(cv2.subtract(opening,closing), cmap='gray')
plt.title('Apertura - Clausura')
plt.axis('off')
plt.suptitle('Apertura y Clausura')

plt.tight_layout() # Adjust rect to make space for suptitle
plt.show()
```

#### 5. (*) Operación de gradiente morfológico: Aplicar el gradiente morfológico (dilatación - erosión). Visualizar los bordes obtenidos mediante esta operación.
![img1](./reporte_imagenes/tp3-2.png)

Fragmento de código:
```python
# Gradiente morfologico
image_path = './img_binarias/bin3.png'
image = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)

kernelToUse = kernel4

# erosion
erosion = cv2.erode(image, kernelToUse, iterations=1)
# dilation
dilation = cv2.dilate(image, kernelToUse, iterations=1)
# opening
opening = cv2.morphologyEx(image, cv2.MORPH_OPEN, kernelToUse)
# closing
closing = cv2.morphologyEx(image, cv2.MORPH_CLOSE, kernelToUse)

## Beucher gradient
beucher_gradient = cv2.subtract(dilation, erosion)

# Gradiente interno
internal_gradient = cv2.subtract(image,erosion)

# Gradiente externo
external_gradient = cv2.subtract(dilation,image)
```

#### 7. (*) Segmentación básica con umbral + morfología: Aplicar umbral, luego apertura y cierre para mejorar el resultado. Ideal como paso previo a una segmentación más elaborada
![img3](image.png)

Fragmento de código:
```python
image_path = "./render0036.png"

image = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)

## Umbralizacion usando otsu
_, binary_image = cv2.threshold(image, 175, 255, cv2.THRESH_BINARY)

## Apertura
kernel = np.ones((5, 5), np.uint8)

opening = cv2.morphologyEx(binary_image, cv2.MORPH_OPEN, kernel)

## Cierre
closing = cv2.morphologyEx(opening, cv2.MORPH_CLOSE, kernel)
```