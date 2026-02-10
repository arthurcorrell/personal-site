---
title: Circulant Matrices and the Convolution Theorem
subtitle: Properties and applications in signal processing
layout: default
date: 2026-02-09
keywords: fourier analysis, eigenvalues, matrix diagonalization
published: true
---



{% katexmm %}

Consider the following cyclic matrix $C_n$

$$\begin{bmatrix}
c_0&c_1&\dots&\dots&\dots&c_{n-1}\\
c_{n-1}&c_0&\dots&\dots&\dots&c_{n-2}\\
\vdots &\ddots & \ddots & & & \vdots \\
\vdots & & \ddots & \ddots & & \vdots \\
c_2 & & & \ddots & c_0 & c_1 \\
c_1 & \dots & \dots & \dots & c_{n-1} & c_0 \\
\end{bmatrix}$$

This class of matrices has a few very elegant properties. For one, they are always diagonalisable by the Fourier matrix such that 
$$C_n = F\Lambda F^{-1}$$

Here, $F$ implements the discrete Fourier transform, and the eigenvalues in $\Lambda$ are computed by taking the discrete Fourier transform of the first row vector. Additionally, any cyclic matrix $C_n$ implements a discrete convolution when applied to a signal $x$. These two properties combine to the convolution theorem. To begin exploring these properties, we will begin by observing that a cyclic matrix $C_n$ can be fully described by only one row vector
$$\vec{c} = \begin{bmatrix}c_0&c_1&\dots&\dots&\dots&c_{n-1}\\\end{bmatrix}$$

where the indices are offset by one in each row. A simple instance of $C_n$ is that where $\vec{c} = \vec{e_n}$, which implements the shift matrix $S_n$

$$\begin{bmatrix}
0&0&\dots&\dots&\dots&1\\
1&0&\dots&\dots&\dots&0\\
\vdots &\ddots & \ddots & & & \vdots \\
\vdots & & \ddots & \ddots & & \vdots \\
0 & & & \ddots & 0 & 0 \\
0 & \dots & \dots & \dots & 1 & 0 \\
\end{bmatrix}$$

This matrix shifts the entries of a vector $\vec{x}$ by one index:
$$S_n \begin{bmatrix}x_0\\x_1\\\vdots\\x_{n-2}\\x_{n-1}\\ \end{bmatrix} = \begin{bmatrix}x_{n-1}\\ x_0\\ \vdots\\ x_{n-3}\\x_{n-2}\\ \end{bmatrix}$$

When we view $\vec{x}$ as a discrete signal, it is quite intuitive to think of this operation as phase-shifting all frequency components of $\vec{x}$ by the same amount. This can be accomplished via the discrete Fourier transform, a powerful tool that can be used to manipulate individual harmonics through complex multiplication. For a discrete signal of length $n$ and indices $k$, the $j$-th frequency component can be found by applying the following operation:

$$
\phi_j = \sum^n_k x_k e^{\frac{2 \pi i}{n} j k}
$$

The result is a set of phasors $\phi_j$, complex number representations of sinusoids, each encoding magnitude and phase for a given harmonic. In vector form, this operation is simply the inner product $\left\langle \vec{x}, \vec{\omega_j} \right\rangle$ where $\vec{\omega_j}$ is the $j$-th Fourier vector:

$$\vec{\omega_j} = \begin{bmatrix} e^{\frac{2 \pi i}{n}j*0} & e^{\frac{2 \pi i}{n}j*1} & \dots & e^{\frac{2 \pi i}{n}j*n}\\ \end{bmatrix}
$$

Each Fourier vector is pairwise orthogonal and can be placed in a matrix as a row vector, yielding the Fourier matrix $F$. In this form, the multiplicative properties of complex numbers can be used to modify the phase and magnitude of individual frequency components. Multiplying each $\phi_j$ by a complex number $\lambda_j = c e ^{k i}$ where $c,k \in \mathbb{N}$ can either amplify its magnitude by $c$ or increment its exponent by $k i$, which phase shifts the phasor along the complex unit circle. To return to the time domain, the Fourier transform is applied again, and we recieve our phase-shifted signal. Thus, the decomposition exists 

$$S_n = F\Lambda F^{-1}$$

where $\Lambda$ is a diagonal matrix holding the corresponding complex number $\lambda_j$. In this form, the row vectors of $F$ are the eigenvectors of $S_n$, and the diagonal entries of $\Lambda$ are the eigenvalues of $S_n$. This result can be generalized to any cyclic matrix $C_n$, because each cyclic matrix can be written as a linear combination of shift/permutation matrices:

$$C_n = c_0 I + c_1 S_n + c_2 S_n^2 + ... + c_{n-1} S_n^{n-1}$$

This property is quite intuitive: exponentiating the shift matrix simply shifts each column vector by one index, allowing one to populate $C_n$ one coefficient at a time. This very naturally leads to the convolution theorem, which states that any convolution of a signal is equivalent to multiplication in the frequency domain. 



{% endkatexmm %}