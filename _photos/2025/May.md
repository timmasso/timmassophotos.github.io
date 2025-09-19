---
layout: null
title: May
category: "2025"
year: 2025
date: 2025-05-01
permalink: /_photos/2025/May/
back_home_text: "Months"
back_home_url: "/_photos/months/"
---
<a class="back-link" href="{{ page.back_home_url }}">{{ page.back_home_text }}</a>
<h1 class="month-title">May</h1>

<!-- Global loading overlay -->
<div id="loading-overlay">
  <div class="loading-message"></div>
</div>

<div class="photo-grid">
  {% assign all_files = site.static_files | where_exp:"file","file.path contains '/assets/photos/2025/May/'" %}
  {% assign photos = "" | split: "" %}
  {% for file in all_files %}
    {% assign ext = file.extname | downcase %}
    {% if ext == ".jpg" or ext == ".jpeg" or ext == ".png" or ext == ".gif" or ext == ".webp" or ext == ".svg" %}
      {% assign photos = photos | push:file %}
    {% endif %}
  {% endfor %}
  {% for file in photos %}
    <div class="photo-item">
      <a href="{{ file.path | relative_url }}" class="glightbox"
         data-description="Loading EXIF..."
         data-image="{{ file.path | relative_url }}">
        <img src="{{ file.path | relative_url }}" alt="{{ page.title }} photo" loading="lazy" />
        <span class="thumb-desc">Loading date...</span>
      </a>
    </div>
  {% endfor %}
</div>

<!-- GLightbox -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/glightbox/dist/css/glightbox.min.css" />
<script src="https://cdn.jsdelivr.net/npm/glightbox/dist/js/glightbox.min.js"></script>

<script type="module">
  import { parse } from "https://cdn.jsdelivr.net/npm/exifr/dist/full.esm.js";

  function flashFired(flashValue) {
    if (flashValue === undefined) return 'Unknown';
    return (flashValue & 1) ? 'Yes' : 'No';
  }

  function meteringModeDesc(mode) {
    const map = {
      0: 'Unknown',
      1: 'Average',
      2: 'Center-weighted average',
      3: 'Spot',
      4: 'Multi-spot',
      5: 'Pattern',
      6: 'Partial',
      255: 'Other'
    };
    return map[mode] || 'Unknown';
  }

  function whiteBalanceDesc(wb) {
    return wb === 0 ? 'Auto' : wb === 1 ? 'Manual' : 'Unknown';
  }

  window.addEventListener('DOMContentLoaded', () => {
    const overlay = document.getElementById('loading-overlay');
    const items = [...document.querySelectorAll('.glightbox[data-image]')];

    // Show overlay and disable clicks globally
    overlay.style.display = 'flex';
    document.body.style.pointerEvents = 'none';

    const promises = items.map(async (anchor) => {
      const imgSrc = anchor.getAttribute('data-image');
      try {
        const response = await fetch(imgSrc);
        const blob = await response.blob();
        const exif = await parse(blob);
        if (exif) {
          const dateTaken = exif.DateTimeOriginal || exif.DateTime || null;
          let dateStr = "Unknown date";
          if (dateTaken) {
            const dt = new Date(dateTaken);
            dateStr = dt.toISOString().slice(0, 16).replace('T', ' ');
          }
          const hoverSpan = anchor.querySelector('.thumb-desc');
          if (hoverSpan) hoverSpan.textContent = "Taken: " + dateStr;

          const cameraMake = exif.Make || 'Unknown';
          const cameraModel = exif.Model || '';
          const exposure = exif.ExposureTime || 'N/A';
          const fnumber = exif.FNumber || 'N/A';
          const iso = exif.ISO || 'N/A';
          const focalLength = exif.FocalLength || 'N/A';
          const flash = flashFired(exif.Flash);
          const whiteBalance = whiteBalanceDesc(exif.WhiteBalance);

          const width = exif.ExifImageWidth || exif.PixelXDimension || exif.ImageWidth || 'N/A';
          const height = exif.ExifImageHeight || exif.PixelYDimension || exif.ImageHeight || 'N/A';
          const resolution = (width !== 'N/A' && height !== 'N/A') ? `${width} × ${height}` : 'N/A';
          const orientation = exif.Orientation || 'Unknown';

          const descriptionHtml = `
            <strong>Date:</strong> ${dateStr}<br/>
            <strong>Camera:</strong> ${cameraMake} ${cameraModel}<br/>
            <strong>Exposure:</strong> ${exposure}s<br/>
            <strong>Aperture (F-Number):</strong> f/${fnumber}<br/>
            <strong>ISO:</strong> ${iso}<br/>
            <strong>Focal Length:</strong> ${focalLength} mm<br/>
            <strong>Flash Fired:</strong> ${flash}<br/>
            <strong>White Balance:</strong> ${whiteBalance}<br/>
            <strong>Resolution:</strong> ${resolution}<br/>
            <strong>Orientation:</strong> ${orientation}
          `;
          anchor.setAttribute('data-description', descriptionHtml);
        }
      } catch {
        const hoverSpan = anchor.querySelector('.thumb-desc');
        if (hoverSpan) hoverSpan.textContent = "Taken: Unknown date";
        anchor.setAttribute('data-description', "No EXIF data available");
      }
    });

    Promise.all(promises).then(() => {
      // Hide overlay and re-enable clicks
      overlay.style.display = 'none';
      document.body.style.pointerEvents = '';
      GLightbox({ selector: '.glightbox', openEffect: 'fade', closeEffect: 'fade', slideEffect: 'slide' });
    }).catch(() => {
      overlay.style.display = 'none';
      document.body.style.pointerEvents = '';
    });
  });
</script>

<style>
  body {
    color: white;
    font-family: monospace;
    font-size: 16px;
    line-height: 1.6;
    margin: 0;
    min-height: 100vh;
    background-image: url('/assets/may.jpg');
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
    position: relative;
  }

  /* Loading overlay */
  #loading-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.85);
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 10000;
  }
  .loading-message {
    color: #39ff14;
    font-size: 1.5rem;
    font-weight: bold;
    font-family: monospace;
  }
  @keyframes dotPulse {
    0%   { content: "Please wait, loading photos"; }
    33%  { content: "Please wait, loading photos."; }
    66%  { content: "Please wait, loading photos.."; }
    100% { content: "Please wait, loading photos..."; }
  }
  .loading-message::after {
    content: "Please wait, loading photos";
    animation: dotPulse 1.5s infinite steps(4, end);
    display: inline-block;
    white-space: pre;
  }

  .back-link {
    color: #39ff14;
    margin-bottom: 1rem;
    display: inline-block;
    margin-left: 31%;
    margin-top: 3.8%;
    font-weight: bold;
  }

  .month-title {
    text-align: center;
    font-size: 2rem;
    margin-top: 0.5rem;
    margin-bottom: 1.5rem;
    color: #39ff14;
  }

  /* Grid */
  .photo-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 12px;
    margin-top: 1rem;
    max-width: calc(100% - 200px);
    margin-left: auto;
    margin-right: auto;
    padding-right: 10px;
  }

  .photo-item {
    position: relative;
    display: inline-block;
  }

  .photo-item img {
    width: 100%;
    height: auto;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.3);
    object-fit: cover;
    display: block;
    cursor: pointer;
  }

  /* Hover text on thumbnail with fade effect */
  .photo-item .thumb-desc {
    position: absolute;
    bottom: 5px;
    left: 5px;
    background: rgba(0,0,0,0.5);
    color: #39ff14;
    font-size: 0.75rem;
    padding: 2px 6px;
    border-radius: 4px;
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease;
  }

  .photo-item:hover .thumb-desc {
    opacity: 1;
    visibility: visible;
  }

  /* GLightbox caption */
  .glightbox-container .gslide-description {
    font-size: 0.9rem !important;
    color: #39ff14 !important;
    background: transparent !important;
    text-shadow: 0 0 2px rgba(0,0,0,0.7);
    padding: 2px 4px !important;
    border-radius: 4px !important;
    position: absolute !important;
    bottom: 10px !important;
    left: 10px !important;
    max-width: 300px !important;
    text-align: left !important;
    z-index: 9999 !important;
    font-family: monospace !important;
  }
</style>
