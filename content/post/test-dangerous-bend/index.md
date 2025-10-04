---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "Test Dangerous Bend"
subtitle: "Where I experiment with different ways to get TeXbook style Dangerous bend sections"
summary: "There is nothing to see here. It will change as I approach and find a solution"
authors: []
tags: []
categories: []
date: 2025-10-03T18:46:29-05:00
lastmod: 2025-10-03T18:46:29-05:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

My attempts to use and customize Hugo blox callouts have not gone well.

I should note that I have not touched CSS since CSS version 2, and what I have is
far more through trial and error than through understanding.

First I will try with regular callouts, using my `alert-advanced` css class, that
inserts a PDF imagine using something like


{{% callout "advanced" %}}
This makes use of `{{/* callout "advanced" */%}}`, and relies on the CSS shown 
the [custom css figure](#fig-custom-css).
Nothing that I put in the highlit line or there abouts seems to have any
impact on the size of the dangerous bend image. I do not know why.
{{% /callout %}}

```scss {title = "alert-advanced class" linenos = true hl_lines = "8" id="fig-custom-css"}
div.alert-advanced > div {
  display: block;
  font-size: 0.8rem;
}

div.alert-advanced > div:first-child::before {
  content: url("/images/dbend.pdf") / "Dangerous Bend";
  width: 1.5rem; // This isn't affect the displayed width of the image.
}

.alert-advanced   {
  color: #000000;
  background-color: #d2d6cb;
  border-color: #1976d2;
}

div.alert-advanced  {
  font-size: smaller
}
```

[That excerpt]((#fig-custom-css)) is the result of me copying relevant parts of a `_callouts.scss`
that I found buried deep inside `blox_bootstrap`.


I can also try with the mechanism I see documented in `callout.html` of passing the name of the image to use,

{{% callout dangerous-bend %}}
For some reason this just goes with the default, and does not make use of the
image.
I do know, however that it is trying to fetch the image because if I typo the name,
I will get a build error.
{{% /callout %}}

Just before the `dbend` shortcode

{{< dbend >}}
Trying the CSS that Google's AI recommended. Let's see how that works.
I am skeptical, but I will continue to give it some test to work with.
And let me add more text, as I am having problems with the exact placement things.

And even a second paragraph in here.
{{< /dbend >}}

Just after the dbend shortcode.

