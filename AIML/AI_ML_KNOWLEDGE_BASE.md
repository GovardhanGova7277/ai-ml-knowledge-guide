## 1. The Bias-Variance Tradeoff
The bias-variance tradeoff is the struggle to find the right balance between a model being too simple and too complex to minimize overall error.

* **Bias:** Error from overly simplistic assumptions. High bias causes underfitting (the model misses the trend).
* **Variance:** Error from extreme sensitivity to small fluctuations in training data. High variance causes overfitting (the model memorizes noise).
* **The Tradeoff:** Decreasing bias increases variance, and vice versa. The goal is to find the sweet spot that minimizes total error.

## 2. Training Data vs. Testing Data
These datasets serve completely separate purposes during the development of a machine learning model.

* **Training Data:** The actual dataset used to teach the model. The algorithm learns its patterns and adjusts its weights based on this data.
* **Testing Data:** A separate, unseen dataset used exclusively to evaluate how well the model generalizes to new, real-world information.

## 3. Gradient Descent vs. SGD vs. Mini-Batch
These are three variations of an optimization algorithm used to minimize a model's error, differing by how much data they look at before updating the model's weights.

* **Gradient Descent (Batch):** Uses the entire dataset to calculate the gradient for a single update. It is highly accurate but very slow and memory-intensive for large data.
* **Stochastic Gradient Descent (SGD):** Uses only one random sample to update weights at each step. It is extremely fast but the updates are highly noisy and erratic.
* **Mini-Batch Gradient Descent:** Uses a small subset of data (e.g., 32 or 64 samples) per update. It balances the stability of Batch GD with the speed of SGD.

## 4. Importance of Feature Scaling and Normalization
Feature scaling shifts and rescales numerical features so they all share a similar scale (e.g., 0 to 1 or a mean of 0).

* **Speeds up convergence:** Gradient descent converges much faster when features are on the same scale, preventing the algorithm from oscillating wildly.
* **Prevents dominance:** It ensures features with naturally larger numbers (like income) do not mathematically overwhelm features with smaller numbers (like age).
* **Required for distance metrics:** Distance-based algorithms (like KNN, K-Means, and SVM) will yield incorrect results if the scales are not normalized.