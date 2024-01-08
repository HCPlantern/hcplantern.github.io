---
title: An Interactive Guide to Flexbox
date: 2023-09-09
draft: false
tags:
  - frontend
  - css
categories:
  - study-notes
featuredImage: https://r2.hcplantern.top/2023/09/10/20230910-205025.gif
---

<!--more-->

{{<admonition quote "Source Article">}}
  - [An Interactive Guide to Flexbox](https://www.joshwcomeau.com/css/interactive-guide-to-flexbox)

{{</admonition >}}

![](https://r2.hcplantern.top/2023/09/10/20230910-205025.gif)

## Introduction

{{<admonition abstract "Layout Algorithm">}}
**Each layout algorithm is designed to solve a specific problem.** The default *Flow* layout is meant to create digital documents; it's essentially the *Microsoft Word* layout algorithm. Headings and paragraphs stack vertically as blocks, while things like text, links, and images sit inconspicuously within these blocks.

While *Flexbox* is all about *flexibility*. We can control whether items grow or shrink, how the extra space is distributed, and more.
{{</admonition >}}

## Flex direction

Flex direction determines the *primary axis*, and the *cross axis* that runs perpendicularly.

-  `flex-direction: row`:  *primary axis* runs horizontally, from left to right.
-  `flex-direction: column` : *primary axis* runs vertically, from top to bottom.

![](https://r2.hcplantern.top/2023/09/10/20230910-203813.png)

## Alignment

![](https://r2.hcplantern.top/2023/09/10/20230910-204914.gif)

{{<admonition note "4 Key Terms to Understand Alignment" >}}

- `justify` — to position something along the *primary axis*.
- `align` — to position something along the *cross axis*.
- `content` — a **group** of “stuff” that can be distributed.
- `items` — **single** items that can be positioned individually.

{{</admonition >}}

### justify-content

We can change how children are distributed **along the primary axis** using the `justify-content` property:

- bunch all the items up in a particular spot (with `flex-start`, `center`, and `flex-end`)
- spread them apart (with `space-between`, `space-around`, and `space-evenly`).

### justify-self

There's no `justify-self` because you cannot adjust one single element across the primary axis. 

> What would it mean for that middle piece to set `justify-self: flex-start`? There's already another piece there!
### align-items

**For the cross axis, We use the `align-items` property:**  

- flex-start, center, flex-end (same as justify-content)
- stretch, baseline (different with justify-content)
- ![](https://r2.hcplantern.top/2023/09/10/20230910-204140.png)
### align-self

![](https://r2.hcplantern.top/2023/09/10/20230910-205405.png)


`align-self` has all the same values as `align-items`. In fact, **they change the exact same thing.** `align-self` changes a single child element.  ` align-items ` is _syntactic sugar_, a convenient shorthand that automatically sets the alignment on all the children at once.

## Hypothetical size

![](https://r2.hcplantern.top/2023/09/10/20230910-210538.png)

- In *Flow* layout, `width` is a _hard constraint_.
- In _Flexbox_, however, the `width` property is more of a **suggestion** than a hard constraint.

{{<admonition type=tip title="CSS mental model" open=true >}}
  {{<admonition type=failure title="wrong model" open=true >}}
  Think of the CSS language as a collection of properties
  {{</admonition >}}

As we've seen, the `width` property behaves differently depending on the layout mode used!

  {{<admonition type=success title="right model" open=true >}}
  CSS as a collection of layout modes. Each layout mode is an algorithm that can implement or redefine each CSS property. We provide an algorithm with our CSS declarations (key/value pairs), and the algorithm decides how to use them.
  {{</admonition >}}
{{</admonition >}}

## Growing and Shrinking

### flex-basis

- In a Flex row, `flex-basis` does **almost the same thing** as `width`. In a Flex column, `flex-basis` does the same thing as `height`.
- `width` will always affect the horizontal size. It doesn't suddenly become `height` when we flip `flex-direction` from `row` to `column`. Same to height. 

![](https://r2.hcplantern.top/2023/09/10/20230910-211414.gif)


### flex-grow

`flex-grow` tells us how to allocate free space to child elements

![](https://r2.hcplantern.top/2023/09/10/20230910-211627.gif)

### flex-shrink

**What if our children are too big for their container?**

![](https://r2.hcplantern.top/2023/09/10/20230910-211858.gif)

Both items shrink, **but they shrink proportionally.** The first child is always 2x the width of the second child.

`flex-shrink` tells us how elements shrink when container is not enough big. To prevent elements from scaling down, set ` flex-shrink ` to 0. 

## Minimum Size

In addition to the _hypothetical_ size, there's another important size that the Flexbox algorithm cares about: _the minimum size_.

- Text inputs have a default minimum size of 170px-200px (it varies between browsers).
- For an element containing text, the minimum width is the length of the _longest unbreakable string of characters._

## Gap

Using `justify-content: space-between` cannot control space between child elements.  `gap` allows us to create space _in-between_ each Flex child. This is great for things like navigation headers:

![](https://r2.hcplantern.top/2023/09/10/20230910-212319.png)

`gap` is a relatively **new addition to the Flexbox language**. 

## Auto margins

- Earlier, we saw how the `flex-grow` property can gobble up any extra space, applying it to a child.
- Auto margins will **gobble up the extra space, and apply it to the element's margin.** It gives us precise control over where to distribute the extra space.

So both do the same thing. But why auto margins? 

{{<admonition type=tip title="Auto Margins" open=true >}}

The auto-margins solution treats the extra space _as a resource_, and deciding exactly where it should go. This solution makes space to become **margin** while the flex-grow solution gives free space to the **body**.

{{</admonition >}}


## Wrapping

- When we set `flex-wrap: wrap`, **items won't shrink below their hypothetical size**.
- Each row has its own **mini flex container** 

### align-content

When elements wraps, we can use it to _align the rows themselves._

![](https://r2.hcplantern.top/2023/09/10/20230910-213221.gif)

{{<admonition abstract "Summary">}}

- `flex-wrap: wrap` gives us two rows of stuff.
- Within each row, `align-items` lets us slide each individual child up or down
- Zooming out, however, we have these two rows within a single Flex context! The cross axis will now intersect *two* rows, not one. And so, we can't move the rows individually, we need to distribute them *as a group*.
- Using our definitions from above, we're dealing with *content*, not *items*. But we're also still talking about the cross axis! And so the property we want is `align-content`.

{{</admonition >}}
   
