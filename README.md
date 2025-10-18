# Autonomous Vehicle - Self-Driving Car Project

## 🚗 Project Overview

This project implements a deep learning model for autonomous vehicle steering angle prediction using a convolutional neural network (CNN). The model processes camera images from a vehicle and predicts the appropriate steering angle to navigate the road.

## 📁 Project Structure

```
AutonomousVehicle/
├── .ipynb_checkpoints/          # Jupyter notebook checkpoints
├── App/                         # Application files
├── images/                      # Project images and visualizations
├── logs/                        # TensorBoard log files
├── models/                      # Saved model files
├── self-driving-car-master/     # Dataset and driving logs
│   ├── IMG/                     # Camera images
│   └── driving_log.csv          # Driving data log
├── model_achitecture.png        # Model architecture diagram
├── projer_finalipynb.ipynb      # Main Jupyter notebook
└── README.md                    # Project documentation
```

## 🛠️ Installation & Setup

### Prerequisites
- Python 3.7+
- TensorFlow 2.x
- OpenCV
- Jupyter Notebook

### Installation Steps

1. **Clone the repository**
```bash
git clone https://github.com/sarataib/AutonomousVehicle.git
cd AutonomousVehicle
```

2. **Create a virtual environment (recommended)**
```bash
python -m venv autonomous_env
source autonomous_env/bin/activate  # On Windows: autonomous_env\Scripts\activate
```

3. **Install required dependencies**
```bash
pip install -r requirements.txt
```

If requirements.txt is not available, install manually:
```bash
pip install tensorflow opencv-python matplotlib pandas numpy scikit-learn imgaug jupyter
```

## 📊 Dataset

The project uses the **Self-Driving Car Master** dataset containing:
- **7,043 driving samples** with camera images
- **Three camera views**: center, left, right
- **Steering angles**, throttle, and speed data
- **Image resolution**: Original images processed to 66x200 pixels

### Data Structure
- `center_*.jpg` - Center camera images
- `left_*.jpg` - Left camera images  
- `right_*.jpg` - Right camera images
- `driving_log.csv` - Driving parameters and image paths


## 🔧 Data Preprocessing

### Image Processing Pipeline:
1. **Color Space Conversion**: BGR to YUV
2. **Gaussian Blur**: 3x3 kernel for noise reduction
3. **Resizing**: 200x66 pixels (model input size)
4. **Normalization**: Pixel values scaled to [0, 1]

### Data Augmentation:
- **Pan**: Random horizontal and vertical translation
- **Zoom**: Random scaling (1.0-1.2x)
- **Brightness**: Random brightness adjustment (0.4-1.2x)
- **Flip**: Horizontal flipping with steering angle inversion

## 🚀 Training

### Training Configuration:
- **Optimizer**: Adam (learning rate: 0.0001)
- **Loss Function**: Mean Squared Error (MSE)
- **Batch Size**: 20
- **Epochs**: 25
- **Train/Validation Split**: 80/20

### Training Process:
```python
history = model.fit(
    batchGen(X_train, Y_train, 20, 1),
    steps_per_epoch=1000,
    epochs=25,
    validation_data=batchGen(X_valid, Y_valid, 20, 0),
    validation_steps=20,
    callbacks=[checkpoint, tensorboard]
)
```

## 📈 Results

### Model Performance:
- **Training Accuracy**: ~78%
- **Validation Accuracy**: ~87% (best epoch)
- **Validation Loss**: 0.0471 (best epoch)

### Best Model Saved:
`cnn-parameters-improvement-07-0.87.model`

## 🎯 Usage

### Running the Jupyter Notebook:
```bash
jupyter notebook projer_finalipynb.ipynb
```

### Making Predictions:
```python
# Load the trained model
model = load_model("models/cnn-parameters-improvement-07-0.87.model")

# Preprocess input image
processed_image = img_preprocess(your_image)

# Make prediction
steering_angle = model.predict(np.expand_dims(processed_image, axis=0))
```

### TensorBoard Visualization:
```bash
tensorboard --logdir logs/
```

## 🔍 Key Insights

1. **Data Balance**: Initial steering data showed imbalance, addressed through augmentation
2. **Color Space**: YUV conversion improved feature extraction
3. **Regularization**: L1 regularization prevented overfitting
4. **Augmentation**: Data augmentation significantly improved model generalization

## 👥 Contributors

- **Sara Taib** - Project Developer
  - GitHub: [@sarataib](https://github.com/sarataib)


**Note**: This project is for educational and research purposes. Always follow safety guidelines when testing autonomous systems.
