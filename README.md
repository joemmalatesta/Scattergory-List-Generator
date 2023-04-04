# Scattergories List Generator
![Preview](/static/preview.gif)

## Remake of [SwellGarfo's](https://swellgarfo.com/scattergories/) using Svelte and Tailwind
I really liked the design of the website and it's very useful for one of my favorite games of all time. I stuck closely to the design, functionality, and even transitions of the original but there were a few things I wanted to add, change, and remove.
### Added features
- Added option for manually changing text sizing
  - Screen size will determine initial size but if it does not fit as intended, you can change the text size to fit all uses.
- Added an animation for time up, as the original just had an alert and would not hide the list.
### Changed Features
- Categories cannot be repeated during the same session. They are removed from the main list.
- Remove the option to add your own categories
  - I feel as though adding your own categories is not in line with the point of the app
  - Moreover, the implementation in the original for adding custom categories was poor
- I changed the phone screen layout to include new list generation as it's own section instead of adding it to the play and remove. 
- Some more small transition, font, and sizing changes.

### Future Changes
- I will potentially bring back the option to add your own categories to only the current list.
- Make this as a PWA
- I was hoping to make a .io game mirroring Scattergories for SvelteHack but that may come in the future.
  - Perhaps making an Async version would be cool where you log on once a day and complete the puzzle or make your own lists. 
    - You join a group and keep a running total of points day over day.
