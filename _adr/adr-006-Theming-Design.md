# ADR 006: Theming & Design

## Changelog
* 2022-03-09: Initial draft

## Status
DRAFT Not Implemented

## Abstract
A branded, well-designed and marketable product is essential for user adoption and user experience.

### Goals
* Responsive CSS with media queries system
* Smallest CSS filesize 
* Base view frame with customizable branding (default, white-label theme)
* Global theming and sizing variables

## Context

### Options Considered
* Tailwind
* Bootstrap
* Material
* No Framework (Pure CSS or SCSS)

## Decision
* Tailwind

Tailwind CSS only needs the base stylesheet file, which amounts up to 27kb making it lighter (unused styles can also be purged).

Bootstrap has four files that are required to include in your project to get the full benefit of the CSS Framework. The total size of these files is 308.25kb including, jQuery, Popper.js, Bootstrap JS, and the main Bootstrap CSS file.

Websites created using Tailwind CSS are much more customizable.	Websites created with Bootstraps are known for their responsiveness and flawless design, but the looks are generic and similar.

Tailwind CSS uses a set of utility classes to create a neat UI with more flexibility and uniqueness. Sites created using Bootstrap follow the generic pattern that makes them look identical. Although Bootstrap and Material can save time upfront with default their default theme, it's easy to recognize a site which uses pre-defined Bootstrap or Material components.

Pure CSS or SCSS fail to implement a styling strategy and thus each designer may end up creating their own approach, this often leads to duplicate and unused styles, even when a single designer is writing CSS classes.

For a unique, custom brand and better optimized CSS file sizes, and to implement a CSS framework strategy, Tailwind will be used. 

## Consequences
Project will be designed using the chosen framework, which will help to manage and organize styles and theming, however designers will need to learn to use Tailwind.

### Backwards Compatibility
N/A

### Positive
* Lightweight CSS Framework.
* Large and growing user base. 
* Not a cookie-cutter design implementation.

### Negative
* Learning curve. 
* More time to create custom theme designs and custom components.

### Neutral
N/A

## Further Discussions

## Test Cases
Experiment with global level and component level css compiling and loading with base pages, themes, and components prior to replicating design system.

## References
* [https://www.npmtrends.com/bootstrap-vs-material-vs-tailwindcss](https://www.npmtrends.com/bootstrap-vs-material-vs-tailwindcss)
* [https://www.geeksforgeeks.org/tailwind-css-vs-bootstrap/](https://www.geeksforgeeks.org/tailwind-css-vs-bootstrap/)