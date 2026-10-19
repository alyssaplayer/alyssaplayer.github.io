---
layout: page
title: Palos Verdes Reef as Refuge Habitat
description: Analysis of the Palos Verdes Reef as a refuge for climate-threatened species.
summary: "Statistical analysis in R of whether Palos Verdes Reef, an artificial reef in the Southern California Bight, acts as a refuge for sea stars and sea urchins threatened by wasting disease and climate change. Used BACIPS models and generalised linear mixed models to compare species densities before, during and after wasting events, in collaboration with the Vantuna Research Group at Occidental College."
img: assets/img/batstars.jpeg
importance: 1
category: scientific research
links:
  - name: GitHub
    url: https://github.com/alyssaplayer/AP-VRG-PVR-RefugiaAR
permalink: /projects/refugia-ar/
---

## The role of Palos Verdes Reef as a refuge habitat for climate-threatened species

This project analyses the role of artificial reefs acting as conservation habitats for climate-threatened species, most notably sea stars and sea urchins, along the Southern California Bight. This project employs multiple analytical approaches, and all data processing and statistical testing were conducted in R. This project was completed in collaboration with the Vantuna Research Group at Occidental College. 


<div class="row">
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid path="assets/img/density_by_depth_stars.png" title="Density of Focal Sea Star Species" class="img-fluid rounded z-depth-1" %}
</div>
<div class="col-sm mt-3 mt-md-0">
{% include figure.liquid path="assets/img/density_urchins.png" title="Density of Focal Urchin Species" class="img-fluid rounded z-depth-1" %}
</div>
</div>
<div class="caption">
Left: Density of focal sea star species. Right: Density of focal urchin species.
</div>


## Analyses

* **Population Metrics:** Generated box plots to visualise the density of focal species across distinct eras (pre-, during-, and post-wasting events).
* **Spatio-temporal Analysis:** Utilised Progressive-Change BACIPs (Before-After-Control-Impact Paired Series) models to assess the effects of human intervention on the surrounding ecosystems.
* **Geographical Visualisation:** Mapped survey locations across the Southern California Bight using site coordinates to provide visual context during data presentation.
* **General Linear Mixed Model** A flexible model approach for analysing nonnormal data with present random effects.

