# OCR Research & Testing Results

This repository contains sample images, OCR response outputs, and research notes collected while testing **[ImgOCR](https://www.imgocr.com/)** with different types of real-world image inputs.

The purpose of this repository is to document observed OCR behavior, processing times, image-quality effects, and potential limitations across different input conditions.

Testing includes:

* Digital text images
* Camera-captured documents
* Screenshots
* Handwritten text
* Blurry images
* Rotated/raw camera images
* Different DPI/resolution samples
* Non-English text
* Different image sizes
* Stylized text
* PDF-to-Word conversion

> **Testing target:** [ImgOCR – AI Image to Text / Online OCR](https://www.imgocr.com/)

---

## Repository Contents

```text
/
├── sample-images/
│   ├── digital/
│   ├── handwritten/
│   ├── camera/
│   ├── screenshots/
│   └── ...
│
├── response-outputs/
│   └── OCR text responses
│
├── research-notes.txt
│
└── README.md
```

The exact folder structure may evolve as additional experiments are added.

---

# Testing Methodology

Samples were tested using a mixture of real-world images, camera captures, screenshots, and reference images.

Tests varied across:

* Image source
* Image quality
* Cropping
* Rotation
* Blur
* Image dimensions
* File size
* DPI
* Text style
* Language
* Handwritten vs. printed text
* Camera images vs. screenshots

Response time was recorded during each test and documented in the research notes.

### Important

These timings are **observed test results**, not guaranteed processing times.

Actual response time may vary depending on:

* Server load
* Network conditions
* Image dimensions
* Image size
* Image complexity
* OCR workload
* Queue/concurrency
* Input characteristics

---

# Response Time Results

## 1. Cropped Camera / Digital Images

| Test Case                | Input Description                                    | Response Time |
| ------------------------ | ---------------------------------------------------- | ------------: |
| Digital image from phone | Cropped digital image taken from phone               |    **46.80s** |
| English camera image     | Cropped English camera image containing digital text |    **58.62s** |
| Urdu blurry image        | Cropped blurry Urdu digital text                     |    **~2 min** |

### Observation

Camera-captured images generally required more processing time than clean digital screenshots.

The Urdu blurry sample also showed substantially longer processing time.

---

# 2. Handwritten Images

| Test Case                 | Input Description                                | Response Time |
| ------------------------- | ------------------------------------------------ | ------------: |
| Handwritten camera sample | Cropped handwritten sample captured using camera |  **~1.5 min** |
| Handwritten blurry sample | Cropped, blurry image with size reduced to 50%   |  **~1.2 min** |

### Observation

Handwritten inputs were among the slower test cases.

Image preprocessing such as cropping and resizing reduced unnecessary image content, but did not necessarily result in proportionally faster processing.

---

# 3. Raw Camera Images — Bulk OCR Processing

These tests used original camera images with minimal or no manual editing.

The samples were:

* Rotated
* Directly captured from a camera
* Not manually cropped
* Generally larger than 4 MB

| Test Case           | Input Description       | Response Time |
| ------------------- | ----------------------- | ------------: |
| Handwritten English | Raw camera image, 4+ MB |    **~4 min** |
| Digital non-English | Raw camera image, 4+ MB |  **~3.8 min** |
| Digital English     | 150 DPI image           |  **~2.1 min** |

### Observation

Raw camera images produced some of the longest processing times in this dataset.

Potential contributing factors include:

* Large image dimensions
* File size
* Rotation
* Camera artifacts
* Background content
* Text positioning
* Image complexity

These factors were not independently controlled in this experiment, so the results should be treated as observational.

---

# 4. Screenshots and Digital Photos

Cleaner digital inputs generally produced faster results.

| Test Case          | Input Description              | Response Time |
| ------------------ | ------------------------------ | ------------: |
| Digital photo      | Digital text photo             |    **21.28s** |
| Stylish text photo | Photo containing stylized text |     **9.50s** |
| Screenshot         | Screen capture containing text |     **6.92s** |

### Observation

Screenshots were among the fastest image-based inputs tested.

This may be related to their relatively clean backgrounds, sharp text boundaries, and absence of camera-related distortion.

---

# 5. DPI / Resolution Tests

Additional tests were performed using different DPI/resolution conditions.

| Test Case          | Input Description | Response Time |
| ------------------ | ----------------- | ------------: |
| Stylish text photo | 300 DPI           |     **9.28s** |
| Screenshot         | 300 DPI           |     **9.24s** |
| Digital photo      | 150 DPI           |    **19.28s** |

### Observation

DPI alone does not appear to explain the observed response-time differences.

Other variables such as:

* Pixel dimensions
* Compression
* Text density
* Image complexity
* Content type

may also influence processing time.

A controlled experiment using the **same source image at different DPI values** would be required to isolate the effect of DPI.

---

# 6. Image Size and Processing Time

One of the research objectives was to investigate whether image size affects OCR response time.

Initial observations suggest that larger and raw camera images can require considerably more processing time.

Examples:

```text
Raw handwritten camera image       ~4 min
Raw digital non-English image      ~3.8 min
Raw 150 DPI digital English        ~2.1 min
Cropped digital samples             < 1 min
```

However, **file size alone should not be treated as the cause**.

Two images with the same file size can have very different:

* Pixel dimensions
* Text density
* Compression
* Background complexity
* Rotation
* Blur
* Language
* Text type

Further controlled testing is required.

---

# 7. 5 MB Image Limit / Long Processing Test

During testing, a **5 MB image** resulted in extremely long processing behavior and eventually produced an error.

### Observed result

> Processing became impractically slow and eventually failed.

This was recorded as an observed test issue rather than a general claim about all 5 MB images.

Further testing would be required to determine whether the behavior was primarily related to:

* File size
* Image dimensions
* Upload time
* Server-side processing
* OCR processing
* Queue time
* Timeout limitations

---

# 8. PDF to Word Conversion

PDF-to-Word conversion was also tested separately.

| Test                  | Response Time |
| --------------------- | ------------: |
| PDF → Word conversion |      **3.9s** |

This particular test completed considerably faster than several of the image-based OCR tests.

---

# Summary of Observations

### Fastest observed tests

```text
PDF → Word conversion       3.9s
Screenshot                  6.92s
Stylish text photo          9.50s
300 DPI stylish photo       9.28s
300 DPI screenshot          9.24s
```

### Slowest observed tests

```text
Raw handwritten camera image       ~4 min
Raw digital non-English image      ~3.8 min
Raw 150 DPI digital English        ~2.1 min
Handwritten camera sample          ~1.5 min
Blurry handwritten image           ~1.2 min
Blurry Urdu digital image          ~2 min
```

---

# Preliminary Findings

Based on the current dataset:

1. **Clean screenshots tended to process quickly.**
2. **Raw camera images could take significantly longer.**
3. **Handwritten samples were among the slower inputs.**
4. **Blurred images showed longer processing times in the tested cases.**
5. **Large/raw camera images may introduce substantial processing overhead.**
6. **DPI alone does not explain the observed processing-time differences.**
7. **Image complexity appears to be an important variable.**
8. **The tested PDF-to-Word conversion completed very quickly.**
9. **A 5 MB test produced an observed long-processing/error condition.**

These findings are **preliminary observations from a limited test dataset** and should not be interpreted as statistically validated benchmarks.

---

# Research Data

The repository includes:

* Original/sample input images
* OCR response text
* Processing-time observations
* Research notes
* Screenshots used during testing

Keeping the input and output together makes it possible to:

* Reproduce individual tests
* Compare OCR results
* Investigate recognition issues
* Compare processing times
* Test future OCR versions
* Evaluate preprocessing techniques
* Track improvements over time

---

# Image & Sample Sources

The repository contains samples from multiple sources.

To keep the research transparent, samples should be considered under the following categories.

## 1. Original / Self-Captured Samples

Some images were created or captured specifically for this research, including:

* Camera photographs
* Handwritten samples
* Digital test images
* Cropped versions of test images
* Resized/compressed versions
* Screenshots created during testing

These samples are used for experimental testing and comparison.

---

## 2. Google Image Search References

Some test images were sourced through **Google Image Search** for research/testing purposes.

These images may belong to their respective original creators or copyright holders.

They are included in this repository strictly as research/test inputs where applicable.

### Important

Google Image Search is a **search/discovery service**, not necessarily the original copyright source of an image.

Where possible, the original source should be identified and credited rather than treating Google as the image owner.

If the original source is known, it should be documented alongside the relevant sample.

Example:

```text
Source: Google Image Search
Original source: [Original Website URL]
Purpose: OCR testing / research
```

---

## 3. Screenshots of Google Image Search

Some samples are screenshots captured from Google Image Search during the research process.

These screenshots are retained to document:

* What image was selected
* How the sample was discovered
* The original search context
* The research/testing process

The screenshot itself should not be interpreted as ownership of the underlying image.

Where the original image source can be identified, the original source should be credited.

---

# Credits

### OCR Platform

All OCR processing discussed in this repository was performed using:

**[ImgOCR](https://www.imgocr.com/)**

ImgOCR provides AI-powered image-to-text OCR processing for images including JPG, PNG, screenshots, handwritten content, and other document images.

### Image Sources

Images in this repository may originate from:

* Original samples created for this research
* Personal camera captures
* Screenshots
* Google Image Search
* Third-party websites discovered through image search

Third-party images remain the property of their respective copyright holders.

Where an original source is known, it should be credited in the relevant sample folder or accompanying notes.

---

# Research Notes

Detailed observations, raw timings, testing comments, and additional notes are maintained in:

```text
research-notes.txt
```

The notes file contains informal observations recorded during testing and should be considered part of the experimental record.

---

# Future Testing

Future experiments should ideally control one variable at a time.

## Image Size

```text
500 KB
1 MB
2 MB
3 MB
4 MB
5 MB
```

## Resolution / DPI

```text
72 DPI
96 DPI
150 DPI
200 DPI
300 DPI
600 DPI
```

## Image Quality

```text
Original
Compressed
Blurred
Sharpened
Grayscale
Enhanced contrast
Noise removed
```

## Image Source

```text
Screenshot
Phone camera
Scanner
Generated digital image
PDF-rendered image
```

## Text Type

```text
Printed English
Printed Urdu
Other languages
Handwritten English
Handwritten Urdu
Stylized fonts
Mixed text
Tables
Forms
Documents
```

---

# Disclaimer

This repository documents experimental OCR testing.

The recorded response times represent the conditions under which the tests were performed. They should **not** be interpreted as guaranteed processing times or universal OCR benchmarks.

Third-party images remain the property of their respective owners.

Where third-party images are used for testing, the repository should maintain source information whenever the original source can be identified.

Additional controlled testing with larger sample sizes is required before making statistically significant performance claims.

---

## Status

**Research Status:** Ongoing

**Current Focus:**

* OCR response-time analysis
* Image-quality impact
* Image-size impact
* Handwriting processing
* DPI/resolution testing
* Camera-image processing
* OCR output comparison
* Reproducible OCR testing
