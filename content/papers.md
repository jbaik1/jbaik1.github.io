---
# Leave the homepage title empty to use the site title
title: ""
date: 2024-12-01
type: landing

design:
  # Default section spacing
  spacing: "6rem"

sections:
  - block: collection
    content:
      title: Papers
      text: ""
      filters:
        folders:
          - publication
        exclude_featured: false
    design:
      view: citation

    design:
      # Choose a layout view
      view: date-title-summary
      # Reduce spacing
      spacing:
        padding: [0, 0, 0, 0]
---
