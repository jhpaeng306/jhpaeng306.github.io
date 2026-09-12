---
layout: cv
permalink: /cv/
title: CV
nav: true
nav_order: 3
cv_pdf: /assets/pdf/CV.pdf # the PDF icon next to the title links here; you can also use an external link
cv_format: rendercv # the CV content lives in _data/cv.yml
description: The sections below are generated from <code>_data/cv.yml</code>.
toc:
  sidebar: right # shown in the right margin on wide screens, hidden otherwise (see _sass/_custom.scss)
---

<!-- Small page extras. The CV itself is generated from _data/cv.yml (see _layouts/cv.liquid). -->

<script>
  // Rename the first card's heading (the plugin hard-codes "Contact Information").
  // This runs before the table of contents is built, so the TOC picks up the new name.
  document.querySelectorAll(".cv .card-title").forEach(function (h) {
    if (h.textContent.trim() === "Contact Information") h.textContent = "Information";
  });

  // Put a "Download CV" link at the top of the table of contents.
  document.addEventListener("DOMContentLoaded", function () {
    var toc = document.getElementById("toc-sidebar");
    if (!toc || document.getElementById("cv-download")) return;
    var a = document.createElement("a");
    a.id = "cv-download";
    a.href = "{{ page.cv_pdf | relative_url }}";
    a.target = "_blank";
    a.rel = "noopener noreferrer";
    a.innerHTML =
      "<span>CV</span>" +
      '<svg viewBox="0 0 24 24" width="15" height="15" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">' +
      '<path d="M12 3v11"/><path d="M8 10l4 4 4-4"/><path d="M5 12v6a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2v-6"/></svg>';
    a.setAttribute("aria-label", "Download CV (PDF)");
    toc.parentNode.insertBefore(a, toc);
  });
</script>
