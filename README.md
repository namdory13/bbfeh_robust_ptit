# BBFEH Robustness Lab

## Overview

This lab is designed for the Labtainers platform and focuses on the robustness analysis of the Bipolar Backward-Forward Echo Hiding (BBFEH) audio steganography technique.

Students will perform multiple signal-processing attacks against audio files containing hidden echo-based information and evaluate whether the hidden pattern can still be detected.

---

# Learning Objectives

After completing this lab, students should be able to:

* Understand the concept of echo hiding in audio steganography
* Understand Bipolar Backward-Forward Echo Hiding (BBFEH)
* Use audio forensic tools such as:

  * ffmpeg
  * sox
* Perform audio signal attacks:

  * MP3 compression
  * low-pass filtering
  * resampling
  * additive noise
* Analyze robustness of hidden audio signals
* Use automated assessment in Labtainers

---

# Platform Requirements

* Ubuntu
* Docker
* Labtainers

---

# Installation

## Method 1 — Using imodule (Recommended)

Copy the lab package into your Labtainers environment and run:

```bash
cd ~/labtainer/trunk
imodule bbfeh-robust.tar.gz
```

Then start the lab:

```bash
labtainer -r bbfeh-robust
```

---

## Method 2 — Manual Installation

Extract the archive into the Labtainers labs directory:

```bash
cd ~/labtainer/trunk/labs
tar -xzvf bbfeh-robust.tar.gz
```

Run the lab:

```bash
cd ~/labtainer/trunk
labtainer -r bbfeh-robust
```

---

# Lab Tasks

Students are provided with:

```text
audio_samples/secret.wav
```

The objective is to evaluate the robustness of BBFEH after several signal-processing attacks.

---

# Suggested Workflow

## Analyze audio file

```bash
ffmpeg -i audio_samples/secret.wav
```

```bash
sox audio_samples/secret.wav -n stat
```

---

## MP3 Compression Attack

```bash
ffmpeg -i audio_samples/secret.wav -b:a 64k mp3_attack.mp3
```

```bash
ffmpeg -i mp3_attack.mp3 mp3_attack.wav
```

---

## Low-pass Filtering Attack

```bash
sox audio_samples/secret.wav filtered.wav lowpass 3000
```

---

## Resampling Attack

```bash
ffmpeg -i audio_samples/secret.wav -ar 22050 resampled.wav
```

---

## Noise Attack

Create a Python script:

```bash
nano noise_attack.py
```

Example:

```python
import soundfile as sf
import numpy as np

x, sr = sf.read("audio_samples/secret.wav")

noise = 0.03 * np.random.randn(len(x))
y = x + noise

sf.write("noise.wav", y, sr)
```

Run:

```bash
python3 noise_attack.py
```

---

## Detection

```bash
python3 detector.py audio_samples/secret.wav
python3 detector.py mp3_attack.wav
python3 detector.py filtered.wav
python3 detector.py resampled.wav
python3 detector.py noise.wav
```

---

# Automated Assessment

The lab automatically checks:

* audio analysis commands
* MP3 attack creation
* low-pass filtering
* resampling
* noise attack creation
* detector execution

To ensure grading works correctly, save shell history:

```bash
history -a
```

Check grading:

```bash
checkwork bbfeh-robust
```

---

# Expected Concepts

Typical results may show:

* BBFEH survives MP3 compression
* BBFEH survives low-pass filtering
* BBFEH survives moderate noise
* BBFEH may fail after resampling

---

# Directory Structure

```text
bbfeh-robust/
├── analyst/
├── config/
├── dockerfiles/
├── docs/
├── instr_config/
├── README.md
└── .gitignore
```

---

# Author

Do Nam

Cybersecurity / Audio Steganography Research
