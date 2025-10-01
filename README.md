# Handwritten Digit Recognition (MNIST) — C++ from Scratch + Browser Demo

A tiny, educational implementation of a feed‑forward neural network that learns to recognize handwritten digits from the MNIST dataset. Training is pure C++ (no ML frameworks), and a lightweight HTML5 canvas demo lets you draw digits in the browser using the trained weights.

---

## ✨ Highlights
- **From scratch**: one hidden layer MLP with sigmoid activations and backpropagation
- **Simple & fast**: minibatch SGD with a few lines of standard C++
- **Transparent**: prints per‑epoch train/test accuracy and training time
- **Interactive**: copy the learned weights into the included `demonstration.html` to try your model in the browser

---

## Network Architecture
```
Input:  28×28 grayscale → flattened to 784 + bias → 785 inputs
Hidden: 30 sigmoid neurons
Output: 10 sigmoid neurons (digit 0–9), choose argmax
Loss/Training: mean‑squared error with sigmoid derivatives, minibatch SGD
```
Hyperparameters (defined in `recognition.cpp`):

| Parameter | Default |
|---|---|
| Epochs | 30 |
| Mini‑batch size | 10 |
| Learning rate | 3.0 |
| Hidden neurons | 30 |

---

## Project Structure
```
.
├── recognition.cpp          # Training/evaluation entry point
├── neural_network.h         # Templated MLP (forward/backward, SGD)
├── data_loader.h            # Reads MNIST IDX files and normalizes to [0,1]
├── timer.h                  # Tiny RAII timer (reports training time)
├── demonstration.html       # HTML5 canvas demo; paste weights to try in browser
└── (generated) Error.csv    # Per‑epoch train/test accuracy (if enabled)
└── (generated) WeightsBiasesJSON.txt  # Saved weights/biases for web demo
```

---

## Getting the Data (MNIST)
1. Download the four IDX files and put them in the project root next to the executable:
   - `train-images.idx3-ubyte`
   - `train-labels.idx1-ubyte`
   - `t10k-images.idx3-ubyte`
   - `t10k-labels.idx1-ubyte`
2. (Optional) Decompress if they come as `.gz` (filenames above are the decompressed names).

> The loader expects exactly these filenames and will normalize pixels to `[0,1]`.

---

## Build
You need a C++17 (or newer) compiler.

**Linux/macOS (Clang or GCC):**
```bash
# From the project folder
c++ -std=c++17 -O3 recognition.cpp -o mnist
# or
g++ -std=c++17 -O3 recognition.cpp -o mnist
```

**Windows (MSVC Developer Prompt):**
```bat
cl /std:c++17 /O2 recognition.cpp
```

---

## Train & Evaluate
```bash
./mnist
```
What you’ll see:
- Per‑epoch **Training** and **Test** accuracy
- A final summary of both
- The total **training time**
- Two files written to disk:
  - `Error.csv` — CSV log of accuracies (one row per epoch)
  - `WeightsBiasesJSON.txt` — JSON dump of the trained network

---

## Try It in the Browser
1. Open `demonstration.html` directly in your browser.
2. By default it contains a `g_neuralNetwork` object with example weights.
3. To try **your** trained model:
   - Open `WeightsBiasesJSON.txt` after training
   - Copy the JSON content and replace the object inside `demonstration.html` (the `g_neuralNetwork = { ... }` part)
   - Save and refresh the page
4. Draw a digit in the large canvas. The right‑hand table shows the top predictions.

> The demo preprocesses your drawing like MNIST: it finds the bounding box, scales to 20×20, centers by mass, and renders into a 28×28 grid before feeding the network.

---

## Customization
- **Change hidden size / epochs / LR**: tweak the constants at the top of `recognition.cpp`.
- **Disable per‑epoch logging**: set `REPORT_ERROR_WHILE_TRAINING()` to `0`.
- **Different activation/loss**: the math lives entirely in `neural_network.h` — easy to experiment.

---

## Troubleshooting
- **“Could not open … for reading.”** — Ensure the four MNIST files are in the same folder you run the program from.
- **Numbers are NaN or training doesn’t improve** — Try a smaller learning rate (e.g., 0.5–1.0) or increase hidden neurons.
- **HTML demo shows zeros** — Make sure you pasted valid JSON into the `g_neuralNetwork` object and reloaded the page.

---

## Acknowledgments
- Dataset: MNIST handwritten digits (Y. LeCun et al.)
- This repo is intentionally minimalist for learning purposes — extend as you like!

---

## License
Choose a license that matches your goals (e.g., MIT for permissive use). If you include a `LICENSE` file, mention it here.
