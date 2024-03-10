## Description
This project is a Svelte application with interactive data visualisation. It shows the use of different colours in the episodes of the TV show "The Colours of Bob Ross" as a Scrollyteller and provides an interactive overview of all paintings and their colours.

## See the result
The result is published here: https://colors-of-bob-ross.vercel.app/

## Installation:
- Clone this Repo
- Go to the directory
- Run npm i to install the dependencies
- Run npm run dev

## Data:
I used this dataset: https://github.com/jwilber/Bob_Ross_Paintings
It contains urls to all the paintings created in the "The Joy of Painting" series, and information about each painting, such as the colours used.
I also calculated the dominant colour for each painting. I used Pyhton for data exploration and analysis. The Juptyer notebooks can be found in src/data.

## Credits:
Data: https://github.com/jwilber/Bob_Ross_Paintings
Scrollyteller component: https://svelte.dev/repl/81194f65fdc74601930df7974fb9ffff?version=3.38.3

## Future improvements:
- Improve tooltip positioning
- Use Tailwind for better CSS handling
- Improve interaction with the interactive painting grid
- Animate changes in the dataviz on scroll
