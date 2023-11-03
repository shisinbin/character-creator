# Character Creation Workshop

In this workshop, you'll build a Sims-style character creation screen.

## Mockups

Desktop:

<img alt="Desktop-sized screenshot of the character creation screen" src="./docs/mockup.png" style="width: 100%;" />

For this workshop, we're only targeting desktops (although if you're so inclined, creating a mobile variant would make a great stretch goal!).

> Protip: If you're reading this in VS Code, open the Command Palette (Cmd + Shift + P) and select “Markdown: Open Preview to the Side”. That way, you can see all the images in this document!

## Setup Instructions

This project uses create-react-app.

Start by installing dependencies:

```
npm install
```

Boot up a dev server:

```
npm run start
```

You should be able to access the application at `http://localhost:3000`.

## Troubleshooting

If you run into problems running a local development server, check out our [Troubleshooting Guide](https://courses.joshwcomeau.com/troubleshooting) on the course platform.

This guide addresses the common `Digital Envelope Routine` error you may have seen.

## Project structure and context

This project is built with React.

**All of the functionality has already been built.** Your job is to add the CSS. You're also allowed to tweak the JSX (HTML) as-needed. But you shouldn't need to fuss with any advanced React stuff.

This project uses **CSS Modules**. CSS modules are ultimately very similar to vanilla CSS, but the classes are applied in JS. Here's an example:

```css
/* Something.module.css */
.wrapper {
  width: 500px;
}
```

```js
/* Something.js */
import styles from './Something.module.css';

function Something() {
  return <div className={styles.wrapper}>I'll be 500px wide!</div>;
}
```

Additionally, a few global styles can be found in `src/index.css`.

## Exercises

### Exercise 1: Fix footer links

Let's start with a small detail: The footer links are unreadable:

<img alt="Side-by-side comparison of the current footer vs. the ideal one" src="./docs/footer-fix.png" style="max-width: 500px" />

> **Solution:** `color: inherit`

### Exercise 2: Layout adjustments

Next, let's tackle the biggest visual issue: the layout.

We have a `MaxWidthWrapper` constraining the max width, but everything is super wide within it.

Our header should be 65% of the available width, and our control-panel column should be 50%.

<img alt="Annotated mockup showing the overall width at 1024px, the header occupying 65%, and the control panels occupying 50%" src="./docs/sizes.png" style="width: 100%;" />

The character (the big illustration) should use fixed positioning, and it should occupy the space cleared by the above width tweaks.

Give the character a minimum height of 500px. On smaller windows, this means the character won't fit in the viewport:

<img alt="Screenshot of a shorter Chrome window, with the character truncated at the knees" src="./docs/short-window.png" style="width: 100%;" />

> NOTE: If you notice at some point that the character SVG disappears, it's likely because it needs to be given an explicit width/height. This is discussed in more depth on the “Solution” page, https://courses.joshwcomeau.com/css-for-js/02-rendering-logic-2/20-character-workshop-solution#collapsed-svgs

> **Solution:** <br/>
> Set `width: 65%` on the max-width-wrapper, `width: 50%` on the control-column.<br/>
> Then for the character, first target the character wrapper, as that contains the svg, and the svg uses `height: 100%` so it will take up all the wrapper. On the wrapper, use `position: fixed` to start the process of fixing its position relative to the viewport. Then we need to give it some kind of dimensions so we can see it, and the idea was to use the layout to help us work out the percentages. So, we want it to start `60%` from the left, because the control-column takes up 50% and so it makes sense for it to start around that mark. But also 15% from the `top` and 15% from the `bottom`, because it felt right. Finally, give it a `min-height` so if it gets squished, it retains at least a certain minimum height.

### Exercise 3: Overflow

Each control panel features a number of customizations. For control panels with too many options, a horizontal scrollbar should be introduced:

<img alt="Close-up screen recording of the overflow area in the control-panel" src="./docs/overflow.gif" style="width: 100%;" />

<br/>

> **Solution:**
> add
> `overflow-x: auto;` and
> `white-space: nowrap;`
> to the `<ButtonRow>` component. This adds a scrollbar to the rows that need it right underneath the buttons. This is almost okay, except that the scrollbar should be on the bottom of the container. So, we use the negative margins trick to expand the button row to take up the container on the left, right and bottom, before adding the same amount of `padding` to the component, thus achieving the desired effect.

### Exercise 4: Perspective decoration

To help add a bit of perspective, a light gray bar should extend across the bottom 40% of the screen:

<img alt="Screenshot of the mockup, showing the light gray bar" src="./docs/mockup.png" style="width: 100%;" />

It should sit behind the avatar (and both the perspective bar and the character should sit below the footer):

<img alt="Screen recording, showing how the character and stripe don't move as the page is scrolled" src="./docs/scroll.gif" style="width: 640px; max-width: 100%" />

You can use the background color `hsl(195deg, 20%, 86%)`.

For bonus points, solve this challenge without setting any z-indexes.

> **Solution:**
> add an empty `<div>`, give it a class so we can apply CSS to it. In the CSS, add `position: fixed`, and the background color, and give it the dimensions we want so that it takes the bottom 40%. Then, the big question is how do we get it to lie underneath the other elements without using `z-index`? Well, we can use DOM order inside the `<main>` component, so put the `<div>` at the beginning. But we also need to do one other thing and that is make the other element in main, the wrapper, a _positioned_ element so that the DOM order thing works. So, we just add a simple `position: relative` to the wrapper element in main, and the desired effect was achieved.

### Exercise 5 (Stretch): Implement a mobile variant

On mobile devices, the cards should stack horizontally, and sit near the bottom of the screen, underneath the character:

<img alt="Screen recording, showing a mobile variant of the application" src="./docs/mobile-variant.gif" style="max-width: 100%" />

**NOTE:** This is a challenging stretch goal! It may require some CSS features we haven't covered yet. This is meant as an extra challenge for advanced students. Feel free to skip it!

## Submissions

**Workshops are submitted through the course platform.** Commit your changes, push them to your fork, and submit the link by clicking the "Complete lesson" button on the workshop page.

If you're not comfortable with Git, you can upload a `.zip` file using Dropbox or Google Drive, and paste a link to the public file instead.

## Note to self

See 'solution' text in every exercise above.

I really liked the trick of using negative margins to expand an inner element to take up full width and bottom of its parent element (effectively negating the padding that had been applied) before then applying an internal padding. That was pretty cool.
