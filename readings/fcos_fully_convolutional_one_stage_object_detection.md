# Paper Info

**Title**: FCOS: Fully Convolutional One-Stage Object Detection

**Year published**: 2019

# Introduction

1). The Drawbacks of anchor-based detectors

* Detection performance is sensitive to the sizes

* The pre-defined anchor boxes hamper the generalization ability of detectors

* Detectors needs to densely place anchor boxes on the input image to get a good performance. This generates a massive number of negative samples which lead to the imbalance between positive and negative samples.

* Anchor boxes involve complicated computation.

2). Ask a question: Can we solve object detection in the per-pixel prediction fashion, analogue to FCN?

* The purpose of doing this is to unify vision tasks (semantic segmentation, keypoint detection, object detection, etc) in one single framework

* The team demonstrate that the simpler FCN-based detector achieves better perforamnce than its anchor-based counterparts.

3). The advantages of the proposed FCOS detection framework:

* Eliminate the computation burden and architecture complexity caused by anchors.

* It can also be used as a Region Proposal Networks (RPNs) in 2-stage detectors and can achieve better performance than the anchor-based RPNs counterparts.

* The proposed detector can be immediately extended to solve other vision tasks.

# The team's approach

1). Reformulate object detection in a per-pixel prediction fashion

