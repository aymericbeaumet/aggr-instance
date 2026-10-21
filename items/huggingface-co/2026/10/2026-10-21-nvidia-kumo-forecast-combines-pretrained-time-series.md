---
title: NVIDIA Kumo Forecast Combines Pretrained Time-Series Representations with Historical Pattern Retrieval and Interpretability
link: https://huggingface.co/blog/nvidia/kumo-forecast
source: huggingface-co
published: 2026-10-21T12:51:24Z
updated: 2026-10-21T12:51:24Z
first_seen: 2026-10-21T14:37:58.044590695Z
content: extracted
html: 2026-10-21-nvidia-kumo-forecast-combines-pretrained-time-series.html
remote_preview:
  url: https://cdn-uploads.huggingface.co/production/uploads/660f5683b710b57b115a5483/pFl0vhvFrc-oZjk5qE8_0.png
---

## Highlights (TL;DR)

NVIDIA Kumo Forecast (formerly known as NV-Tesseract Forecasting) is a multi-variate time-series forecasting model that combines a pretrained Transformer backbone with Domain-Aware Representation and Retrieval (DARR), a context-enhanced forecasting mode that blends direct model predictions with patterns retrieved from historical data. This includes interpretability of the results. Built on the MOMENT-1-large backbone, Kumo Forecast uses an approximately 340-million-parameter base encoder plus a trainable forecasting head.

The library provides a DataFrame-first API, configurable forecast horizons, optional historical context for retrieval, and an interpretability framework that produces horizon-specific explanations without modifying the underlying forecaster.

Kumo-Forecast is available under [Apache 2.0 license](https://www.apache.org/licenses/LICENSE-2.0).

Model code: [NVIDIA/Kumo-TS](https://github.com/NVIDIA/Kumo-TS)\
 Model weights: [nvidia/Kumo-Forecast](https://huggingface.co/nvidia/Kumo-Forecast)

## The Shift to Context-Enhanced Time-Series Forecasting

A forecasting system can draw on two different sources of information: representations learned during pretraining and historical observations available for the current application. Kumo-Forecast brings these together through a pretrained forecasting model and an optional retrieval pathway over historical context.

In its direct forecasting path, Kumo-Forecast uses a MOMENT-based Transformer and a trained forecasting head to predict future values. In Domain Aware Representation and Retrieval (DARR) mode, it also retrieves related patterns from historical data using a nearest-neighbor approach and combines that information with the direct forecast.

This creates a distinction between the model’s input window and its retrieval context. The input window supplies recent observations for the forecast; a separate historical DataFrame can supply additional context for pattern retrieval. Developers can adjust how strongly the final prediction relies on each pathway through the library’s configuration options.

Further information on DARR mode can be found in this published paper, [Memory-Augmented Forecasting: Scalability and Generalization Across Temporal Domains](https://ieeexplore.ieee.org/abstract/document/11402442)

## How Kumo Forecast Works

Kumo-Forecast combines four main components: a pretrained temporal encoder, a trainable forecasting head, historical-pattern retrieval mechanism and an interpretability framework.

**Pretrained Temporal Representations**: The model uses the MOMENT-1-large Transformer encoder as its backbone. The architecture keeps encoder and embedder weights frozen while training the forecasting head, separating the pretrained representation model from the task-specific prediction layer.

**Direct Forecasting**: The forecasting head maps the model’s representations to future values for the requested horizon. For horizons beyond the model’s native capability, there is support for autoregressive extension. This distinguishes a directly generated forecast from a longer forecast that extends predictions recursively.

**Domain-Aware Representation and Retrieval**: DARR augments direct forecasting with k-nearest-neighbor retrieval from historical context data. This process combines direct forecasts with historical-pattern retrieval, while the SDK accepts the historical context through a separate context\_df argument.

**Interpretability**: Kumo-Forecasting also includes a model-agnostic interpretability framework. When enabled, it produces an explanation bundle containing lag and feature attributions per forecast horizon, semantic-flow diagnostics, trajectory-stability diagnostics, and optional embedding Integrated Gradients.These outputs accompany the forecast rather than requiring a different forecasting architecture.

Further details on interpretability can be found in this paper, [NVExplain: Explaining Time Series Forecasting with Latent Trajectory Analysis and Structure-Preserving Surrogates](https://arxiv.org/abs/2608.25080)

## Limitations

Data must be numeric, temporal data. Supported input sequence lengths include 256, 512, 1024, and 2048 observations. Inputs consist of a timestamp column and one or more numeric value columns, represented as a pandas DataFrame or a CSV/JSON input. When leveraging DARR mode, the dimensions of the context must be similar to the context of the input.

## Demo: Predictive Maintenance

The [Factory Operations (FOX) Blueprint](https://developer.nvidia.com/factory-operations-blueprint-early-access) includes Early Access to the Predictive Maintenance module which leverages Kumo Anomaly and Kumo Forecast. It analyzes real-time data to predict future anomalies. Your browser does not support the video tag.

## Demo: Platform Integration

Please click on the image below to access the session from GTC San Jose 2026

[![alt text](https://huggingface.co/nvidia/Kumo-Forecast/resolve/main/images/image-gtc-2026.png)](https://www.nvidia.com/en-us/on-demand/session/gtc26-s81871/)

## Start Building with Kumo Forecast

For an application-specific evaluation, compare the direct and retrieval-enhanced configurations on the same held-out forecasting windows. Validate forecast quality across the intended horizons and operating conditions, consistent with NVIDIA’s requirement for use-case-specific testing

Model code: [NVIDIA/Kumo-TS](https://github.com/NVIDIA/Kumo-TS)\
 Model weights: [nvidia/Kumo-Forecast](https://huggingface.co/nvidia/Kumo-Forecast)

The Kumo-TS repository additionally provides an example for fine-tuning Kumo-Forecast on a user-supplied CSV. Developers therefore have both a pretrained inference workflow and a documented starting point for adapting the model to their own data.
