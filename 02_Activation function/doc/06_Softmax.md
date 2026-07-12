# Softmax Activation Function

## Definition

The **Softmax activation function** converts a vector of real-valued numbers (**logits**) into a **probability distribution**.

Unlike **Sigmoid**, which predicts the probability of each class independently, **Softmax** computes probabilities for **all classes simultaneously**, ensuring that the probabilities sum to **1**.

Because of this property, Softmax is the standard activation function used in the **output layer of multi-class classification** problems.

Given a vector of logits

$$
\mathbf{z}=[z_1,z_2,\ldots,z_K],
$$

where:

- \(K\) = total number of classes
- \(z_i\) = logit (raw score) for class \(i\)

the Softmax probability for class \(i\) is

$$
P(y=i)=
\frac{e^{z_i}}
{\sum_{j=1}^{K}e^{z_j}}.
$$

where

- \(e^{z_i}\) is the exponential of the logit.
- The denominator normalizes all outputs.

Therefore,

$$
\sum_{i=1}^{K}P(y=i)=1.
$$

The outputs can therefore be interpreted as **probabilities**.

---

## How It Works

Suppose the final layer of a neural network produces the logits

$$
z=[2.0,\;1.0,\;0.1].
$$

## Step 1: Compute Exponentials

$$
e^2=7.389
$$

$$
e^1=2.718
$$

$$
e^{0.1}=1.105
$$

---

## Step 2: Compute the Sum

$$
7.389+2.718+1.105=11.212
$$

---

## Step 3: Normalize

### Class 1

$$
P_1=\frac{7.389}{11.212}=0.659
$$

### Class 2

$$
P_2=\frac{2.718}{11.212}=0.242
$$

### Class 3

$$
P_3=\frac{1.105}{11.212}=0.099
$$

The resulting probability distribution is

$$
[0.659,\;0.242,\;0.099].
$$

Notice that

$$
0.659+0.242+0.099=1.
$$

Since **Class 1** has the highest probability, it becomes the predicted class.

---

### Example

Imagine an image classifier predicting one of three animals.

### Network Output (Logits)

| Animal | Logit |
|---------|------:|
| Cat | 3.5 |
| Dog | 1.2 |
| Bird | -0.5 |

After applying Softmax:

| Animal | Probability |
|---------|------------:|
| Cat | 0.89 |
| Dog | 0.09 |
| Bird | 0.02 |

### Interpretation

- **89%** → Cat
- **9%** → Dog
- **2%** → Bird

The predicted class is **Cat**.

---

## Why Exponentials?

Softmax uses the exponential function because it:

- Makes every output positive.
- Amplifies larger logits more than smaller ones.
- Produces a smooth probability distribution.
- Is differentiable, enabling backpropagation.

### Example

Suppose the logits are

$$
[5,\;1,\;0].
$$

Exponentials become

$$
[148.4,\;2.72,\;1].
$$

The largest logit dominates, producing a much higher probability after normalization.

---

## Mathematical Properties

## 1. Output Range

Each probability satisfies

$$
0<P_i<1.
$$

---

## 2. Probabilities Sum to One

$$
\sum_{i=1}^{K}P_i=1.
$$

---

## 3. Monotonicity

A larger logit always produces a larger Softmax probability.

---

## 4. Differentiable

Softmax is fully differentiable, making it suitable for gradient-based optimization.

---

## Advantages

## 1. Produces Valid Probability Distributions

Outputs can be directly interpreted as probabilities.

Example

$$
[0.80,\;0.15,\;0.05]
$$

means

- **80%** confidence in Class 1
- **15%** confidence in Class 2
- **5%** confidence in Class 3

---

## 2. Ideal for Multi-Class Classification

Softmax naturally handles problems where **exactly one class is correct**.

Examples include:

- Handwritten digit recognition (0–9)
- Image classification
- Animal recognition
- Language identification
- Sentiment classification

---

## 3. Differentiable

Softmax integrates seamlessly with:

- Gradient Descent
- Backpropagation

---

## 4. Works with Cross-Entropy Loss

Softmax is almost always paired with **Categorical Cross-Entropy Loss**.

For the true class \(y\),

$$
L=-\log(P_y).
$$

This combination provides:

- Stable gradients
- Efficient optimization
- Faster convergence

---

## Disadvantages

## 1. Sensitive to Large Logits

Exponentials grow very rapidly.

For example,

$$
e^{1000}
$$

is too large to represent numerically.

To avoid overflow, implementations use the **Numerically Stable Softmax**:

$$
P_i=
\frac{e^{z_i-\max(z)}}
{\sum_{j=1}^{K}e^{z_j-\max(z)}}.
$$

Subtracting the maximum logit does **not** change the probabilities but greatly improves numerical stability.

---

## 2. Assumes Classes Are Mutually Exclusive

Softmax assumes **only one class is correct**.

This makes it unsuitable for **multi-label classification**.

### Example

An image containing both:

- Dog
- Cat

should allow both labels simultaneously.

In this case, **Sigmoid** is preferred because each class is predicted independently.

---

## 3. Competitive Outputs

Increasing the probability of one class automatically decreases the probabilities of the others because

$$
\sum_i P_i=1.
$$

The classes compete with each other.

---

## Applications

Softmax is widely used in:

- Multi-Class Image Classification
- Object Recognition
- OCR (Optical Character Recognition)
- Language Identification
- Speech Recognition
- Machine Translation
- Named Entity Recognition (NER)
- Text Classification

---



## Softmax vs ReLU

| Feature | ReLU | Softmax |
|---------|------|----------|
| Purpose | Hidden Layer Activation | Output Layer Activation |
| Output Range | [0,∞) | (0,1) |
| Produces Probabilities |   No |   Yes |
| Hidden Layers | Yes | No |
| Output Layer | Rarely | Multi-Class Classification |

---

## Softmax vs Sigmoid vs Tanh

| Feature | Sigmoid | Tanh | Softmax |
|---------|---------|------|----------|
| Output Range | (0,1) | (-1,1) | (0,1) |
| Zero-Centered |   No |   Yes |   No |
| Hidden Layers | Rarely | Sometimes | No |
| Output Layer | Binary Classification | Rarely Used | Multi-Class Classification |
| Outputs Sum to 1 |   No |   No |   Yes |

---

## When to Use Softmax

Use Softmax when:

- Exactly **one class** is correct.
- The task is **multi-class classification**.
- You want output probabilities that sum to **1**.
- You are using **Categorical Cross-Entropy Loss**.

Examples:

- MNIST Digit Recognition (10 classes)
- CIFAR-10 Image Classification
- ImageNet (1000 classes)
- Language Detection
- Intent Classification in NLP

---

## Key Takeaways

- Softmax converts **logits** into a **normalized probability distribution**.

$$
\boxed{
P(y=i)=
\frac{e^{z_i}}
{\sum_{j=1}^{K}e^{z_j}}
}
$$

- Every probability satisfies

$$
0<P_i<1.
$$

- The probabilities always sum to

$$
\boxed{\sum_{i=1}^{K}P_i=1.}
$$

- Softmax is the standard activation function for **multi-class classification**.
- It is almost always paired with **Categorical Cross-Entropy Loss**.
- For **binary** or **multi-label** classification, **Sigmoid** is generally preferred instead of Softmax.

> **Rule of Thumb**
>
> - **Hidden Layers:** ReLU, Leaky ReLU, GELU, Swish
> - **Binary Classification:** Sigmoid
> - **Multi-Label Classification:** Sigmoid
> - **Multi-Class Classification (Exactly One Class):** Softmax