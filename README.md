# Touying ![logo](https://github.com/touying-typ/touying/assets/34951714/2aa394d3-2319-4572-aef7-ed3c14b09846)

Touying (投影 in chinese, /tóuyǐng/, meaning projection) is a more efficient package for creating presentation slides in Typst.

Compared to Polylux, it employs a more object-oriented writing style, capable of simulating a mutable global singleton. Additionally, Touying does not rely on `locate` and `counter` for implementing `#pause`, thus offering better performance, albeit with certain limitations.

**Warning: It is under development, and the API may change at any time.**

## Dynamic slides

We can export `example.pdfpc` file by command `typst query --root . ./examples/example.typ --field value --one "<pdfpc-file>" > ./examples/example.pdfpc`

```typst
#import "../lib.typ": s, pause, utils, states, pdfpc, themes

#let s = themes.metropolis.register(s, aspect-ratio: "16-9", footer: [Custom footer])
#let s = (s.methods.enable-transparent-cover)(self: s)
#let s = (s.methods.append-preamble)(self: s, pdfpc.config(
  duration-minutes: 30,
  start-time: datetime(hour: 14, minute: 10, second: 0),
  end-time: datetime(hour: 14, minute: 40, second: 0),
  last-minutes: 5,
  note-font-size: 12,
  disable-markdown: false,
  default-transition: (
    type: "push",
    duration-seconds: 2,
    angle: ltr,
    alignment: "vertical",
    direction: "inward",
  ),
))
// #let s = (s.methods.enable-handout-mode)(self: s)
#let (init, slide, touying-outline) = utils.methods(s)
#show: init

// simple animations
#slide[
  a simple #pause dynamic

  #pause
  
  slide.
]

// complex animations
#slide(setting: body => {
  set text(fill: blue)
  body
}, repeat: 3, self => [
  #let (uncover, only) = utils.methods(self)

  in subslide #self.subslide

  test #uncover("2-")[uncover] function

  test #only("2-")[only] function

  #pause

  and paused text.
])

// multiple pages for one slide
#slide[
  #lorem(200)

  test multiple pages
]

// appendix by freezing last-slide-number
#let s = (s.methods.appendix)(self: s)
#let (slide,) = utils.methods(s)

#slide[
  appendix
]
```

![image](https://github.com/touying-typ/touying/assets/34951714/b635ab7f-ed5e-4e80-922b-fb7c7fb8fcdf)


## Themes

```typst
#import "../lib.typ": s, pause, utils, states, pdfpc, themes

#let s = themes.metropolis.register(s, aspect-ratio: "16-9", footer: [Custom footer])
#let s = (s.methods.enable-transparent-cover)(self: s)
#let (init, slide, title-slide, new-section-slide, focus-slide, touying-outline, alert) = utils.methods(s)
#show: init

#set text(font: "Fira Sans", weight: "light", size: 20pt)
#show math.equation: set text(font: "Fira Math")
#set strong(delta: 100)
#set par(justify: true)

#title-slide(
  author: [Authors],
  title: [Title],
  subtitle: [Subtitle],
  date: [Date],
  extra: [Extra],
)

#slide(title: [Table of contents])[
  #touying-outline()
]

#slide(title: [A long long long long long long long long long long long long long long long long long long long long long long long long Title])[
  A slide with some maths:
  $ x_(n+1) = (x_n + a/x_n) / 2 $

  #lorem(200)
]

#new-section-slide[First section]

#slide[
  A slide without a title but with #alert[important] infos
]

#new-section-slide[Second section]

#focus-slide[
  Wake up!
]

// simple animations
#slide[
  a simple #pause dynamic slide with #alert[alert]

  #pause
  
  text.
]

// appendix by freezing last-slide-number
#let s = (s.methods.appendix)(self: s)
#let (slide, new-section-slide) = utils.methods(s)

#new-section-slide[Appendix]

#slide[
  appendix
]
```

![image](https://github.com/touying-typ/touying/assets/34951714/f08dc948-68b4-45d6-8e87-53ca3fc9912c)


## Acknowledgements

Thank you to...

- [@andreasKroepelin](https://github.com/andreasKroepelin) for the `polylux` package
- [@Enivex](https://github.com/Enivex) for the `metropolis` theme
- [@ntjess](https://github.com/ntjess) for contributing to `fit-to-height` and `fit-to-width`
