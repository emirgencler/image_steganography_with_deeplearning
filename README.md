# Deep Learning Image Steganography with VAE

A deep learning based image steganography system using a Variational Autoencoder (VAE) architecture to hide and recover secret messages inside images.

## Overview

This project demonstrates how deep learning can be used for secure and invisible image steganography.

Unlike classical steganography methods such as LSB (Least Significant Bit), this approach hides information inside the latent representation of the image instead of directly modifying image pixels.

The system consists of:

* Encoder Network
* Decoder Network
* Revealer Network

The model learns both:

* reconstructing the image
* recovering the hidden message correctly

---

## Architecture

### Encoder

The encoder compresses a 64×64 RGB image into a latent representation using convolutional layers.

```python
Conv2d(3 → 64)
Conv2d(64 → 128)
Conv2d(128 → 256)
```

The encoder produces:

* Mean vector (μ)
* Log variance vector (log σ²)

---

### Latent Space

The latent vector is sampled using the reparameterization trick:

```python
std = torch.exp(0.5 * logvar)
eps = torch.randn_like(std)
z = mu + std * eps
```

The secret message bits are combined with the latent representation before decoding.

---

### Decoder

The decoder reconstructs the image using transposed convolution layers and produces the stego image.

Goal:

* preserve image quality
* hide the message invisibly

---

### Revealer Network

The revealer network extracts the hidden binary message from the generated stego image.

Recovered bits are converted back into text form.

---

## Message Encoding

Example secret message:

```text
HI
```

ASCII binary conversion:

```text
H → 01001000
I → 01001001
```

The message is converted into binary bits before training.

---

## Training

The model is trained using three losses:

```text
Total Loss = Image Loss + Message Loss + KL Divergence
```

### Loss Function

```python
loss = image_loss + MESSAGE_WEIGHT * message_loss + BETA * kl
```

### Image Reconstruction Loss

```python
nn.MSELoss()
```

### Message Recovery Loss

```python
nn.BCEWithLogitsLoss()
```

### Optimizer

```python
AdamW
```

Learning rate:

```python
8e-4
```

Scheduler:

```python
StepLR(step_size=1000, gamma=0.8)
```

Training setup:

* 10,000 training steps
* Single-image proof of concept

---

## Evaluation Metrics

The project uses:

* PSNR (Peak Signal-to-Noise Ratio)
* SSIM (Structural Similarity Index Measure)

### Results

| Metric | Result   |
| ------ | -------- |
| PSNR   | 58.74 dB |
| SSIM   | 0.999    |

The generated stego image is visually almost identical to the original image.

---

## Technologies Used

* Python
* PyTorch
* TorchMetrics
* OpenCV
* Matplotlib
* Google Colab

---

## Project Workflow

1. Upload image
2. Convert secret message into binary
3. Encode image into latent space
4. Combine latent vector with secret message
5. Generate stego image
6. Recover hidden message using revealer network
7. Evaluate image quality using PSNR and SSIM

---

## Example Output

```text
Original message: HI
Extracted message: HI
```

---

## Future Improvements

* Multi-image dataset training
* Larger hidden messages
* Adversarial steganalysis resistance
* Perceptual loss integration
* Real-world dataset evaluation
* More advanced encoder-decoder architectures

---

## Author

Emir Gençler
