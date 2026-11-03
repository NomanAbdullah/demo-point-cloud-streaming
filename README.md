# Point Cloud Streaming with Latency-Driven Adaptation

**A Google Colab demonstration using real MVUB `andrew9` point-cloud frames and Draco compression.**

Based on:

> Freeman, A. C., et al. (2026). *Point Cloud Streaming with Latency-Driven Implicit Adaptation using MoQ.* NOSSDAV '26. https://doi.org/10.1145/3798065.3798074

## Overview

Can two viewers with different delay requirements reconstruct different levels of detail from the **same source point-cloud recording**, without selecting a fixed quality level?

This notebook explores that question by dividing real captured point-cloud frames into independently decodable pieces, encoding each piece with **Draco**, and evaluating which complete pieces would arrive under modeled network-capacity constraints and delivery deadlines.

**Important distinction:** Draco compression and reconstruction operate on real data. **MoQ/QUIC transport is simulated**, not implemented as a live network connection. The results are an educational demonstration of the paper's mechanism, **not a reproduction of its transport performance**.

## Demonstrated pipeline

1. Read real frames and RGB colors from the Microsoft Voxelized Upper Bodies (MVUB) `andrew9` sequence.
2. Randomly divide each frame into ten **non-overlapping** point subsets.
3. Independently **encode each subset with Draco** using `DracoPy`.
4. Simulate bandwidth-limited, chunk-based delivery with **50 ms, 100 ms, and 500 ms** deadlines.
5. **Decode only complete objects delivered by their deadline** and reconstruct each frame.
6. Compare the proportion of points delivered, number of pieces, and frame stalls.
7. Show an optional, simplified **important-region-first** partitioning demonstration.


## Requirements

- A Google account and [Google Colab](https://colab.research.google.com/).
- Access to the real [MVUB `andrew9.zip` archive](https://plenodb.jpeg.org/pc/microsoft/andrew9.zip), subject to the dataset provider's terms.
- Sufficient Google Drive storage for the ZIP archive.

The notebook installs `DracoPy`, `plyfile`, and `plotly` and uses NumPy, pandas, and Matplotlib in Colab.

## Run in Google Colab

1. Download `andrew9.zip` from the [official dataset URL](https://plenodb.jpeg.org/pc/microsoft/andrew9.zip) and place it at:

   ```text
   MyDrive/Colab Notebooks/CSI5353/andrew9.zip
   ```

2. Open [`Point_Cloud_Streaming_with_Latency_Driven_Adaptation.ipynb`](Point_Cloud_Streaming_with_Latency_Driven_Adaptation.ipynb) in Google Colab.
3. In the notebook, confirm this configuration matches your Drive location:

   ```python
   ZIP_PATH = Path("/content/drive/MyDrive/Colab Notebooks/CSI5353/andrew9.zip")
   ```

4. Select **Runtime → Run all** and allow Drive access.
5. Examine the partition images, saved `.drc` files, delivery comparison, and reconstructed frames.

**Data handling:** The notebook reads a selected group of frames **directly from the ZIP**; it does not need to extract the full dataset. If the ZIP is not found, it stops with a clear error. **No dummy dataset is used.**


