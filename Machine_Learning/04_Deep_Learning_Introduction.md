# Deep Learning (DL) Introduction

## What is Deep Learning?

**Deep Learning** is a subset of Machine Learning that uses **artificial neural networks** with many layers (hence "deep") to learn representations of data.

Unlike traditional ML where features are hand-engineered, deep learning **automatically discovers** features from raw data (images, text, audio).

```
Artificial Intelligence
    └── Machine Learning
            └── Deep Learning
                    └── Neural Networks → CNNs, RNNs, Transformers, LLMs
```

### Why Deep Learning?
- Handles unstructured data (images, text, audio, video)
- Learns automatically without feature engineering
- Achieves human-level accuracy on many tasks
- Scales well with more data and compute

---

## 1. Artificial Neural Networks (ANN)

A neural network mimics the human brain — it consists of layers of interconnected **neurons** (nodes).

### Structure

```
Input Layer → Hidden Layer(s) → Output Layer
```

Each connection has a **weight**. During training, weights are adjusted to minimize errors.

### Key Components

| Component         | Description                                          |
| ----------------- | ---------------------------------------------------- |
| **Neuron**        | Basic computational unit — computes weighted sum + activation |
| **Layer**         | Group of neurons                                     |
| **Weights**       | Parameters the network learns                        |
| **Bias**          | Extra parameter that shifts the activation           |
| **Activation**    | Non-linear function that allows complex patterns     |

### Forward Propagation (Simplified)

```
Input x → [Weight × x + Bias] → Activation Function → Output
```

### Building a Neural Network with Keras (TensorFlow)

```python
import tensorflow as tf
from tensorflow import keras

# Simple ANN for classification
model = keras.Sequential([
    keras.layers.Dense(64, activation='relu', input_shape=(10,)),
    keras.layers.Dense(32, activation='relu'),
    keras.layers.Dropout(0.2),
    keras.layers.Dense(1, activation='sigmoid')  # Binary classification
])

model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)

model.summary()
```

---

## 2. Activation Functions

Activation functions introduce non-linearity — without them, the network is just a linear model.

| Function     | Formula                        | Use Case                        |
| ------------ | ------------------------------ | ------------------------------- |
| **ReLU**     | `max(0, x)`                    | Hidden layers (most common)     |
| **Sigmoid**  | `1 / (1 + e^-x)`              | Binary classification output    |
| **Softmax**  | `e^xi / Σe^xj`                | Multi-class classification      |
| **Tanh**     | `(e^x - e^-x) / (e^x + e^-x)` | RNNs (range: -1 to 1)          |
| **Leaky ReLU**| `max(0.01x, x)`               | Prevents dying ReLU problem     |

```python
import tensorflow as tf
import numpy as np

# ReLU
relu = tf.keras.activations.relu
print(relu(tf.constant([-2.0, 0.0, 2.0])))   # [0. 0. 2.]

# Sigmoid
sigmoid = tf.keras.activations.sigmoid
print(sigmoid(tf.constant([0.0, 2.0, -2.0]))) # [0.5 0.88 0.12]
```

---

## 3. Loss Functions

The **loss function** measures how wrong the model's predictions are.

| Task                     | Loss Function              | Keras Name                    |
| ------------------------ | -------------------------- | ----------------------------- |
| Binary Classification    | Binary Cross-Entropy       | `binary_crossentropy`         |
| Multi-class              | Categorical Cross-Entropy  | `categorical_crossentropy`    |
| Regression               | Mean Squared Error         | `mean_squared_error`          |
| Regression (robust)      | Mean Absolute Error        | `mean_absolute_error`         |
| Multi-label              | BCE per label              | `binary_crossentropy`         |

---

## 4. Optimizers

Optimizers adjust weights to minimize the loss function using **gradient descent**.

| Optimizer   | Description                                        | When to Use           |
| ----------- | -------------------------------------------------- | --------------------- |
| **SGD**     | Basic gradient descent — slow but stable           | Simple problems       |
| **Adam**    | Adaptive learning rate — fast and widely used      | Most cases (default)  |
| **RMSProp** | Good for RNNs and noisy data                       | Recurrent networks    |
| **AdaGrad** | Adapts per-parameter learning rate                 | Sparse data           |

```python
model.compile(
    optimizer=tf.keras.optimizers.Adam(learning_rate=0.001),
    loss='categorical_crossentropy',
    metrics=['accuracy']
)
```

---

## 5. Training a Deep Learning Model

### Full Training Pipeline

```python
import tensorflow as tf
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

# Sample data (binary classification)
X = np.random.randn(1000, 10)
y = (X[:, 0] + X[:, 1] > 0).astype(int)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Scale features
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test  = scaler.transform(X_test)

# Build model
model = tf.keras.Sequential([
    tf.keras.layers.Dense(64, activation='relu', input_shape=(10,)),
    tf.keras.layers.Dense(32, activation='relu'),
    tf.keras.layers.Dropout(0.3),
    tf.keras.layers.Dense(1, activation='sigmoid')
])

model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])

# Callbacks
early_stop = tf.keras.callbacks.EarlyStopping(patience=5, restore_best_weights=True)

# Train
history = model.fit(
    X_train, y_train,
    validation_split=0.2,
    epochs=50,
    batch_size=32,
    callbacks=[early_stop],
    verbose=1
)

# Evaluate
loss, accuracy = model.evaluate(X_test, y_test)
print(f"Test Accuracy: {accuracy:.4f}")
```

### Plot Training History

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 4))

plt.subplot(1, 2, 1)
plt.plot(history.history['loss'], label='Train Loss')
plt.plot(history.history['val_loss'], label='Val Loss')
plt.title('Loss Over Epochs')
plt.legend()

plt.subplot(1, 2, 2)
plt.plot(history.history['accuracy'], label='Train Accuracy')
plt.plot(history.history['val_accuracy'], label='Val Accuracy')
plt.title('Accuracy Over Epochs')
plt.legend()

plt.tight_layout()
plt.show()
```

---

## 6. Convolutional Neural Networks (CNN)

**CNNs** are specialized networks for image data. They use convolutional layers to automatically detect features (edges, shapes, objects).

```
Input Image → Conv2D → MaxPooling → Conv2D → MaxPooling → Flatten → Dense → Output
```

```python
import tensorflow as tf

# CNN for MNIST digit classification
model = tf.keras.Sequential([
    tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(28,28,1)),
    tf.keras.layers.MaxPooling2D((2,2)),
    tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D((2,2)),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dropout(0.3),
    tf.keras.layers.Dense(10, activation='softmax')  # 10 classes
])

model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])

# Load MNIST
(X_train, y_train), (X_test, y_test) = tf.keras.datasets.mnist.load_data()
X_train = X_train[..., tf.newaxis] / 255.0
X_test  = X_test[..., tf.newaxis] / 255.0

model.fit(X_train, y_train, epochs=5, validation_split=0.1)
print(f"Test Accuracy: {model.evaluate(X_test, y_test)[1]:.4f}")
```

---

## 7. Recurrent Neural Networks (RNN/LSTM)

**RNNs** process sequential data (time series, text, audio).
**LSTM** (Long Short-Term Memory) solves the vanishing gradient problem of basic RNNs.

```python
import tensorflow as tf

# LSTM for time series
model = tf.keras.Sequential([
    tf.keras.layers.LSTM(64, return_sequences=True, input_shape=(30, 1)),
    tf.keras.layers.LSTM(32),
    tf.keras.layers.Dense(1)  # Predict next value
])

model.compile(optimizer='adam', loss='mse')
```

---

## 8. Transfer Learning

**Transfer Learning** uses a pre-trained model (trained on millions of images) as a starting point, saving time and compute.

```python
import tensorflow as tf

# Load pre-trained MobileNetV2 (ImageNet weights)
base_model = tf.keras.applications.MobileNetV2(
    weights='imagenet',
    include_top=False,        # Remove the top classification layer
    input_shape=(224, 224, 3)
)

base_model.trainable = False   # Freeze base model weights

# Add custom classification head
model = tf.keras.Sequential([
    base_model,
    tf.keras.layers.GlobalAveragePooling2D(),
    tf.keras.layers.Dense(128, activation='relu'),
    tf.keras.layers.Dropout(0.3),
    tf.keras.layers.Dense(5, activation='softmax')  # 5 custom classes
])

model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])
```

---

## 🎯 Student Tasks – Module 21: Deep Learning

### Task 1: Build Your First ANN (Easy)
**Objective**: Build and train a neural network from scratch.

**Instructions**:
Using `sklearn.datasets.load_iris()`:
1. Preprocess: StandardScaler + OneHotEncoder for the target.
2. Build an ANN: Input(4) → Dense(16, relu) → Dense(8, relu) → Dense(3, softmax).
3. Compile with Adam, categorical_crossentropy.
4. Train for 50 epochs with 20% validation split.
5. Plot loss and accuracy curves.
6. Evaluate on test set.

**Expected Output**:
```
Model: Sequential
Total params: 251

Epoch 1/50 - loss: 1.1042 - accuracy: 0.4167
...
Epoch 50/50 - loss: 0.0823 - accuracy: 0.9750

Test Accuracy: 0.9667
```

---

### Task 2: CNN for Image Classification (Medium)
**Objective**: Build a CNN to classify handwritten digits.

**Instructions**:
Using `tf.keras.datasets.mnist`:
1. Load and preprocess: normalize to [0,1], reshape to (28,28,1).
2. Build a CNN:
   - Conv2D(32, 3×3, relu)
   - MaxPooling(2×2)
   - Conv2D(64, 3×3, relu)
   - MaxPooling(2×2)
   - Flatten → Dense(128, relu) → Dropout(0.3) → Dense(10, softmax)
3. Train for 10 epochs, batch_size=64.
4. Evaluate on test set.
5. Show 5 misclassified images.

**Expected Output**:
```
Test Loss: 0.042
Test Accuracy: 0.9869 (98.7%)

5 Misclassified Samples shown with true and predicted labels.
```

---

### Task 3: Transfer Learning for Custom Classification (Challenge)
**Objective**: Fine-tune a pre-trained model on custom data.

**Instructions**:
1. Use `tf.keras.applications.VGG16` or `MobileNetV2` as base model.
2. Download a small dataset (e.g., CIFAR-10 or a custom image folder).
3. **Phase 1 - Feature Extraction**: Freeze base, train only head.
4. **Phase 2 - Fine-tuning**: Unfreeze top layers and train with smaller learning rate.
5. Compare Phase 1 vs Phase 2 accuracy.
6. Use `ImageDataGenerator` for data augmentation.
7. Save final model, load it, and run inference on 5 new images.

**Expected Output**:
```
Phase 1 (Feature Extraction):
  Train Acc: 0.82  |  Val Acc: 0.79

Phase 2 (Fine-tuned):
  Train Acc: 0.91  |  Val Acc: 0.88  ← Improved!

Model saved to 'transfer_model.h5'

Inference on 5 test images:
  Image 1: Predicted = cat (confidence: 0.92)
  Image 2: Predicted = dog (confidence: 0.88)
...
```

---
