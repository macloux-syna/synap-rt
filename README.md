<div align="center">

SynapRT
===========================
<h3>Real-time AI pipelines with Python</h3>

[![Documentation](https://img.shields.io/badge/docs-latest-brightgreen.svg?style=flat)](https://developer.synaptics.com/)
[![python](https://img.shields.io/badge/python-3.10-brightgreen)](https://www.python.org/downloads/release/python-3123/)
[![version](https://img.shields.io/badge/release-0.0.2.preview-yellow)](./)
[![license](https://img.shields.io/badge/license-Apache%202-blue)](./LICENSE)


[Hardware](https://www.synaptics.com/products/embedded-processors/astra-machina-foundation-series)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[Models](https://developer.synaptics.com/models?operator=AND)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[Examples](./examples/)&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;[Documentation](https://developer.synaptics.com/)
</div>
<hr>


## Overview

The SynapRT Python package allows you to run real-time AI pipelines on your Synaptics Astra board in just a few lines of code. Built on the official [SyNAP Python API](https://github.com/synaptics-synap/synap-python), SynapRT leverages NPU acceleration for efficient, low-latency inference.


## Getting started

### Installation

[Optional] Create a venv to manage dependencies:
```bash
python3 -m venv .venv --system-site-packages
source .venv/bin/activate
```
> [!NOTE]
> `synap-rt` requires python GStreamer bindings from your board's default Python installation. If using a virtual environment, these must be included via `--system-site-packages`.


Next, install the [latest release](https://github.com/spal-synaptics/SyNAP-Infer-RT/releases) directly on your Astra board:

```bash
pip install -e synap-rt
```

### Simple Object Detection Pipeline

A SynapRT pipeline requires three key components: a task, a SyNAP model, and an input source. Additionally, it is recommended to define a handler function to process inference results or integrate them into a broader workflow.

```python
"""A simple object detection pipeline example."""

import json
import sys
 
from synapRT.pipelines import pipeline
 
 
def main():
 
    output_file = sys.argv[2] if len(sys.argv) > 2 else None
 
    def handle_results(results, inference_time):
        print(f"Inference Time: {inference_time:.0f} ms")
 
    pipe = pipeline(
        task="object-detection",
        model="/usr/share/synap/models/object_detection/body_pose/model/yolov8s-pose/model.synap",
        save_file=output_file,
    )
 
    print(f"Starting Object Detection Stream.")
    if output_file:
        print(f"Recording to: {output_file} (Press Ctrl+C to save and exit)")
 
    pipe(sys.argv[1])
 
 
if __name__ == "__main__":
    main()
```

Run the above pipeline with:
```bash
python3 simple.py <input>
```
`<input>` can be image(s), or a video source like camera, MP4 file, or RTSP stream. 

> [!TIP]
> To adapt this code for other tasks (such as image classification), simply update the `task` and `model` parameters.

### More Examples

Explore the [examples](examples/) directory for additional pipeline use cases, such as:

* Running a pipeline in a separate thread to avoid blocking execution.
* Polling a pipeline for results instead of using a handler function.

For application-level implementations using SynapRT pipelines, check out the [official Astra examples repository](https://github.com/synaptics-synap/examples).

