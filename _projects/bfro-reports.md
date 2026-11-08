---
name: Where and When Bigfoot Reports Cluster
title: Where and When Bigfoot Reports Cluster
tools: [Python, Altair, Vega-Lite]
description: Report counts by state and season in the BFRO archive.
permalink: /bfro-reports/
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
---

# Where and when Bigfoot reports cluster

The Bigfoot Field Researchers Organization (BFRO) archive records reported encounters. These charts describe the archive. Differences in population, reporting behavior, and data collection can affect the counts.

<div class="mb-4">
{% include elements/button.html link="https://github.com/UIUC-iSchool-DataViz/is445_data/blob/main/bfro_reports_fall2022.csv" text="The Data" %}
{% include elements/button.html link="https://github.com/eternalxzxzz/eternalxzxzz.github.io/blob/main/python_notebooks/bfro_reports.ipynb" text="The Analysis" %}
</div>

## 1. Which states have the most reports?

Each horizontal bar shows the number of archived reports for one of the ten states with the most records. State is nominal on the vertical axis, and report count is quantitative on the horizontal axis. Sorting the bars by count makes the ranking easy to scan, and a single teal color keeps the focus on bar length. In Python, I removed rows missing a report ID or state, deduplicated by report ID, counted records by state, and kept the ten states with the highest totals. Washington leads with 563 reports, followed by California with 402.

<div id="bfro-state-chart" role="group" aria-label="Horizontal bar chart of the ten states with the most BFRO reports" style="width:100%"></div>

## 2. How do counts vary by season?

The heatmap shows report counts for those same states by recorded season. Season is ordinal on the horizontal axis, state is nominal on the vertical axis, and the quantitative count is encoded by a sequential blue scale. Darker cells indicate more reports. In Python, I excluded records without a known season, grouped the remaining records by state and season for all reports and for each classification, and filled combinations with zero reports. With all classes selected, Washington in summer is the darkest cell, with 222 reports.

<p><strong>Try the dropdown.</strong> Select a report classification to see how the seasonal pattern changes. The state order and color scale stay fixed, so you can compare the same cell across classifications. Hover over a cell for its exact count.</p>

<div id="bfro-season-chart" role="group" aria-label="Heatmap of BFRO report counts by state and season with a classification dropdown" style="width:100%"></div>

<script>
  document.addEventListener("DOMContentLoaded", () => {
    const charts = [
      ["bfro-state-chart", "{{ '/assets/json/bfro-state-counts.json' | relative_url }}"],
      ["bfro-season-chart", "{{ '/assets/json/bfro-season-counts.json' | relative_url }}"]
    ];

    for (const [id, spec] of charts) {
      vegaEmbed("#" + id, spec, { actions: false, renderer: "svg" }).catch((error) => {
        document.getElementById(id).textContent = "The chart could not be loaded.";
        console.error(error);
      });
    }
  });
</script>
