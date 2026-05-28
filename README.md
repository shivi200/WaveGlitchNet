# WaveGlitchNet
CNN-based classifier for 22 categories of LIGO detector glitches using the GravitySpy dataset — 92% test accuracy
WaveGlitchNet — Gravitational Wave Glitch Classification
A convolutional neural network for classifying transient noise artefacts ("glitches") in LIGO gravitational-wave detector data, built using the GravitySpy dataset.
Developed as part of a research internship at Spartificial Innovations Pvt. Ltd.

Background
Gravitational-wave detectors like LIGO are susceptible to transient noise artefacts that can mimic or obscure real astrophysical signals. Correctly identifying and classifying these glitches is essential for maintaining data quality and ensuring the reliability of GW detections. The GravitySpy project provides labelled spectrogram images of 22 distinct glitch categories for this purpose.
Manual classification by human experts is slow and error-prone. WaveGlitchNet automates this task using a compact CNN trained directly on glitch spectrograms.

Dataset

Source: GravitySpy on Kaggle
Classes: 22 glitch categories (e.g. Blip, Koi Fish, Scattered Light, Violin Mode, Whistle, Chirp...)
Split: 22,348 training / 4,800 validation / 4,720 test images
Format: Grayscale spectrograms, resized to 384×384×1
Challenge: Significant class imbalance — addressed using class weighting


Model Architecture
WaveGlitchNet is a custom CNN built from scratch:
Input (384×384×1)
→ Conv2D (32 filters, 5×5) + ReLU
→ MaxPooling2D
→ Dropout (0.2)
→ Conv2D (64 filters, 5×5) + ReLU
→ MaxPooling2D
→ Dropout (0.2)
→ Flatten
→ Dense (128) + ReLU
→ Dense (22) + Softmax

Total trainable parameters: ~70.9 million
Optimizer: Adam
Loss function: Sparse Categorical Crossentropy
Batch size: 64 | Epochs: 5

The architecture was selected after systematic hyperparameter tuning across multiple configurations.

Results
MetricScoreOverall test accuracy92%Weighted avg F1-score0.92Best class (Power Line, Violin Mode)~0.99 F1Hardest class (Wandering Line, Paired Doves)~0.47 F1 (low support)
The model generalises well to unseen data with no overfitting — confirmed by evaluating on a held-out test set with no information leakage from training.

Tech Stack

Python
TensorFlow / Keras
OpenCV
NumPy, Pandas
Matplotlib, Seaborn
Scikit-learn (evaluation metrics)
Google Colab / Kaggle Notebooks (training environment)


Limitations & Future Work

Performance on underrepresented classes (Wandering Line, Paired Doves, None of the Above) remains below 50% — a known consequence of class imbalance that class weighting alone does not fully resolve
Transfer learning (e.g. fine-tuning a pretrained ResNet or EfficientNet) is a natural next step
A feedback loop for retraining on new misclassified examples would improve robustness over time


Context
This project sits within the broader effort to improve data quality pipelines for gravitational-wave astronomy. Reliable glitch classification directly impacts the sensitivity of GW searches and the astrophysical inference that follows. Tools like WaveGlitchNet contribute to the pipeline that will be critical for next-generation detectors such as LISA and the Einstein Telescope.

Author
Shivika Lamba — MSc Astrophysics, Cardiff University
Research interests: gravitational-wave data analysis, waveform modelling, EMRI science, next-generation detectors
