# Academic Homepage Design

## Purpose

Create a concise, English-language academic homepage for Sitan Yan's PhD applications. The page should let a potential supervisor understand the candidate's identity, research direction, strongest project, publications, and contact route within about one minute.

## Scope

This iteration rebuilds only the homepage of the GitHub Pages repository. It includes the SFF reflective micro-hole project as a featured homepage entry, but does not create the `/projects/sff-micropore/` detail page. The project link will use that future route.

All writes are confined to `E:\Abroad_Application\Sitan-Yan.github.io`. The CV, manuscript, and source figures outside the repository are read-only inputs; selected files or derived web assets may be copied into the repository.

## Visual Direction

Use a research-first editorial style:

- White and soft off-white surfaces with charcoal text and restrained academic blue links.
- Generous whitespace, thin rules, and minimal borders instead of decorative cards.
- A serif display face for major headings and a clean system sans-serif stack for body text.
- No gradients, animated backgrounds, parallax, or ornamental motion.
- A generated, low-contrast micro-hole metrology visual will appear as a decorative hero background. It must not be presented as experimental evidence.
- The featured research image will come from the author's real manuscript figures and will carry a descriptive caption.

## Information Architecture

### Header

A compact sticky header contains the name on the left and anchor navigation on the right: About, Research, Publications, Education, and CV. On narrow screens it becomes an accessible menu button with a simple vertical menu.

### Hero and About

The hero uses a two-column layout. The primary column contains:

- Sitan Yan
- M.Eng. candidate in Mechanical Engineering at Huazhong University of Science and Technology
- A two-to-three-sentence research statement focused on robotic perception and control, automated optical metrology, motion planning, and microscopic 3-D reconstruction
- A compact Research Interests list
- Email, GitHub, Google Scholar placeholder, and CV links

The secondary column contains a clearly labeled portrait placeholder that can be replaced later. The generated metrology artwork is used only as a quiet background layer and remains secondary to the text and portrait.

### Selected Research

This is the page's dominant content block. It uses a responsive media-and-copy composition: research imagery on the left and project content on the right. On mobile, the image appears above the text.

The entry includes:

- Title: Reliability-Aware Shape-from-Focus for 3-D Measurement of Reflective Micro-Holes
- Authors: Sitan Yan, Wei Xu, Lingxiao Zeng, and Wenlong Li
- Status: Major Revision, IEEE Transactions on Instrumentation and Measurement
- A short problem-method-outcome summary
- Three highlighted results: 21.5 micrometre mean absolute diameter deviation, 1.277 degree mean absolute axis-angle deviation, and 12.3 micrometre point-cloud MAE on 32 evaluation holes
- Project link to `/projects/sff-micropore/`
- Video placeholder shown only if a usable local video is identified; otherwise omitted to avoid a dead link

### Publications and Manuscripts

Use a compact numbered list. The SFF manuscript appears first with its revision status. The 2023 ICUAS air-ground carrier platform paper appears second with its DOI link. Patent information remains out of the homepage in this iteration to keep the page focused.

### Education

Show HUST M.Eng. and B.Eng. entries with dates, GPA, location, and Distinguished Graduate recognition. Use a simple timeline-like alignment without decorative icons.

### Contact and Footer

End with a concise invitation for PhD, research, and collaboration conversations. Use `sitan@hust.edu.cn` as the primary email. The footer contains the current year and GitHub Pages attribution.

## Content and Placeholder Policy

Verified facts from the supplied CV and manuscript will be used directly. Unknown or unverified items will be explicit placeholders rather than invented facts. These include the portrait, Google Scholar profile URL, and any optional personal tagline not supported by the supplied materials.

## File Structure

The implementation will use:

- `index.html` for semantic content and metadata
- `style.css` for the complete responsive visual system
- `script.js` for the mobile navigation, active-section state, and current year
- `assets/images/` for the portrait placeholder, generated decorative visual, and optimized manuscript-derived image
- `assets/documents/` for the copied CV

No framework, package manager, or build step will be introduced.

## Accessibility and Responsive Behavior

- Semantic landmarks and ordered heading hierarchy
- Visible keyboard focus states
- Descriptive alternative text and captions for meaningful images
- Decorative imagery hidden from assistive technologies
- Sufficient text and link contrast
- Touch targets of at least 44 pixels where practical
- No horizontal overflow at phone widths
- Reduced-motion-safe behavior; no essential motion

## Verification

After implementation:

1. Check all local assets, anchors, document links, and the future project URL.
2. Serve the repository locally and inspect desktop and phone layouts in a browser.
3. Verify keyboard navigation, mobile menu behavior, image alternative text, and focus visibility.
4. Confirm that the generated visual is present in the finished page and that real research figures are not visually misrepresented.
5. Confirm that no files outside the homepage repository were modified.
