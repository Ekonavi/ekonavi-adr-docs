# ADR 006: Theming & Design

## Changelog
* 2022-03-09: Initial draft

## Status
DRAFT Not Implemented

## Abstract
A regionally branded, well-designed and marketable product is essential for user adoption and user experience.

### Goals
* Responsive CSS using media query targets
* Smallest CSS filesize 
* Base view frame with customizable branding (default, white-label theme)
* Theme colors/fonts/sizing organized as global variables

## Context

### Options Considered
* Tailwind
* Bootstrap
* Materialize
* No Framework (Pure CSS or SCSS)

## Decision
* Tailwind

Tailwind CSS only needs the base stylesheet file, which amounts up to 27kb, making the lighter of options (unused styles can also be purged).

Bootstrap has four files that are required for inclusion in projects to get the full benefit of the CSS Framework. The total size of these files is 308kb including, jQuery, Popper.js, Bootstrap JS, and the main Bootstrap CSS file. Quite a lot of overhead out of the box.

Websites created using Tailwind CSS are much more customizable.	Websites created with Bootstraps are known for their responsiveness and flawless design, but the looks are generic and similar.

Tailwind CSS uses a set of utility classes to create a UI with more flexibility and uniqueness. Sites created using Bootstrap follow the generic pattern that makes them look identical. Although Bootstrap and Materialize can save time upfront with their default themes, it's easy to recognize a site which uses these pre-defined components.

Pure CSS or SCSS fail to implement a styling strategy and thus each designer may end up creating their own approach, this often leads to duplicate and unused styles, even when a single designer is writing these CSS classes.

For a unique, custom brand, better optimized CSS file sizes, and to implement an organized CSS framework strategy, Tailwind will be used. 

## Consequences

### Backwards Compatibility
If required, pure CSS could be used alongside tailwind.

### Positive
* Lightweight CSS Framework
* Methodology for organizing styles and theming
* Tailwind adoption growing, large user base
* Not a cookie-cutter design implementation

### Negative
* Learning curve designers would need to learn Tailwind or use their own custom CSS
* More time required to create custom theme designs and custom components

### Neutral
N/A

## Further Discussions

## Test Cases
Experiment with global level and component level css compiling and loading with base pages, themes, and components prior to replicating the design system throughout project.

## References
* [https://www.npmtrends.com/bootstrap-vs-material-vs-tailwindcss](https://www.npmtrends.com/bootstrap-vs-material-vs-tailwindcss)
* [https://www.geeksforgeeks.org/tailwind-css-vs-bootstrap/](https://www.geeksforgeeks.org/tailwind-css-vs-bootstrap/)