# Desk Puzzle 2D — Texture Prompts (ChatGPT / DALL-E fallback)

The Gemini API key on this machine returned a hard quota-0 error for both
Nano Banana 2 (`gemini-3.1-flash-image-preview`) and Nano Banana Pro
(`gemini-3-pro-image`) — the free tier has no image-generation quota and
billing isn't enabled, so no images were generated or downloaded.

**How to use:** For each texture below, paste the **Rules for every image**
block first, then the texture's own prompt right after it, as one message.
Save the result with the exact filename shown and drop it into
`projects/Desk-Puzzle-2D/assets/textures/`.

---

## Rules for every image (paste this before each prompt)

```
Rules for this image: The object must fill the ENTIRE image frame edge-to-edge (full-bleed) — zero surrounding background, zero margins, no border, no drop shadow outside the object, no checkerboard, and NO transparency. The image edges ARE the object's edges. Photorealistic, top-down square-on view (camera directly overhead, 0 degree perspective), evenly lit with no harsh highlights, no text or writing anywhere, no hands, no other objects. Warm neutral tones — absolutely no blue tint on paper items. Subtle realistic material grain.
```

The one exception is **slide.png** (prompt 7), which needs a pure-white
backdrop so the clear glass reads as glass — its prompt says so explicitly.

**If ChatGPT gives you a bordered/shadowed image:** reply with
"crop to the object only, edge-to-edge, no background visible" and it will
usually fix it in one step.

---

## 1. sticky.png (1024x1024)
```
A blank pastel yellow sticky note (Post-it) filling the whole frame edge-to-edge. A slight natural curl shading at the bottom edge of the paper is acceptable, but the curl stays inside the frame. Subtle paper fiber grain. Square, 1024x1024.
```

## 2. sticky-pink.png (1024x1024)
```
A blank soft pastel pink sticky note (Post-it) filling the whole frame edge-to-edge. A slight natural curl shading at the bottom edge of the paper is acceptable, but the curl stays inside the frame. Subtle paper fiber grain. Square, 1024x1024.
```

## 3. sticky-green.png (1024x1024)
```
A blank soft pastel sage-green sticky note (Post-it) filling the whole frame edge-to-edge. A slight natural curl shading at the bottom edge of the paper is acceptable, but the curl stays inside the frame. Subtle paper fiber grain. Square, 1024x1024.
```

## 4. sticky-orange.png (1024x1024)
```
A blank soft pastel orange sticky note (Post-it) filling the whole frame edge-to-edge. A slight natural curl shading at the bottom edge of the paper is acceptable, but the curl stays inside the frame. Subtle paper fiber grain. Square, 1024x1024.
```

## 5. card.png (4:3-ish, e.g. 1024x768)
```
A blank white ruled index card filling the whole frame edge-to-edge. One red horizontal rule line near the top, then faint thin blue-gray horizontal ruled lines spaced evenly down the rest — lines only, no text or writing. Slightly warm off-white card stock with subtle paper grain. 4:3 aspect ratio, approximately 1024x768.
```

## 6. paper.png (square or letter-ish, e.g. 1024x1024)
```
A blank sheet of slightly textured off-white notebook/letter paper filling the whole frame edge-to-edge. No ruled lines, no watermark. Warm neutral off-white (not bright white, not blue-white), subtle visible paper fiber texture and very faint grain. Square, 1024x1024.
```

## 7. slide.png (1024x1024) — EXCEPTION: white backdrop allowed
```
A glass microscope slide photographed from directly above on a pure white backdrop, the slide large and centered, spanning nearly the full frame. Exception to the full-bleed rule: keep the backdrop pure flat white with no shadows, no gradient, no texture — the white IS what the clear glass reads against. The slide is mostly clear transparent glass with a subtle glassy sheen; one short end (about the last fifth) is a frosted white translucent writing area with nothing written on it. Clean clinical laboratory look. Square, 1024x1024.
```

## 8. film.png (1024x1024)
```
A blank dark X-ray film sheet filling the whole frame edge-to-edge. Near-black with a subtle glossy sheen and very faint soft highlight reflections, plus a subtly lighter smooth header strip along the top edge of the film (blank, nothing printed on it). Square, 1024x1024.
```

## 9. desk.jpg (wide, e.g. 1300x550 or larger)
```
A warm mid-brown wood desk surface filling the entire frame edge-to-edge. Uniform, calm wood grain that is subtly varied but not busy — no knots, no strong grain patterns, no objects. Consistent tone and lighting edge to edge so the image can be scaled/stretched to a wide horizontal banner (~1300x550) without obvious seams. Warm neutral brown.
```

## 10. blotter.png (optional, generate last)
```
A dark brown-black leather desk blotter surface filling the entire frame edge-to-edge. Fine realistic leather grain, subtle even sheen, no stitching, no objects. Consistent tone across the whole frame so it tiles cleanly. Warm dark neutral tone (not blue-black). Square, 1024x1024.
```

---

### After generating
1. Save each file with the exact name above into `assets/textures/`.
2. Check the edges: the object should touch all four sides of the image
   (except slide.png). If you can see any background, margin, shadow, or
   checkerboard around the object, use the troubleshooting line above.
3. If any file is very large, downscale so the longest edge is ~1024px.

That's it — the game finds the files by name on the next reload. No
manifest editing, no registration step. (`manifest.json` in this folder
is left over from an older setup and is ignored now.)


---

## New in round 5

### photo.png
Full-bleed base texture (same rules as above — the object touches all four
sides): a blank glossy photo print seen from above, white border on all
sides with a slightly wider bottom strip, EMPTY gray-beige photo window (no
picture content — the game draws the picture in). Soft studio light, no
text anywhere.

### rx.png
Full-bleed: a blank prescription-pad sheet seen from above, slightly warm
white paper, a printed "Rx" glyph in the top-left corner, a thin rule line
under the (empty) header, otherwise completely empty. No handwriting, no
other text.

### sticky-2.png / sticky-3.png
Full-bleed: same yellow sticky note as sticky.png but with clearly
different corner curl, wear, and lighting so the three read as different
physical notes.

### paper-2.png
Full-bleed: same small paper sheet as paper.png with different crumple and
edge wear.

---

## Overlay sprites (assets/textures/overlays/) — RETIRED

**As of round 9, the game no longer reads any overlay file.** The
corner-fold and tape decorations on paper pieces were removed (they read
as visual clutter rather than realism), and `game.js` no longer probes
`overlays/tape-1.png`, `overlays/tape-2.png`, or `overlays/fold-1.png`.
The files below are left on disk for reference only — dropping them into
`assets/textures/overlays/` no longer does anything. The prompts are kept
here in case the look ever comes back.

These are DIFFERENT from the base textures: transparency is REQUIRED.
ChatGPT image generation supports transparent backgrounds — ask for
"isolated on a transparent background, PNG with alpha".

### overlays/tape-1.png and overlays/tape-2.png
A short strip of translucent adhesive tape, photographed straight-on,
isolated on a TRANSPARENT background (PNG with alpha). Slightly crinkled,
matte edges. Two different strips, roughly 3:1 wide.

### overlays/fold-1.png
A folded-paper corner (dog-ear) shadow overlay, isolated on a TRANSPARENT
background: just the triangular fold highlight and its soft shadow, nothing
else. Used composited over paper pieces.
