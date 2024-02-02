---
title: "Rendering a Jupyter Notebook in Markdown for Static Site Generation"
date: 2024-02-01
draft: false
---

**Spoiler Alert** This was originally a jupyter notebook!

While taking a recent Machine Learning course for my masters, I had 
the idea of how nice it would be if I did a cool notebook to be able
to post it to my personal blog with almost no work involved. 

And, to my pleasant suprise, it was pretty simple! Here are the steps to get there:

## What you can render

### Code blocks

```js
// this is some javascript
var x = 5;
console.log(x);
```

```python
# now some python
for el in l:
    print(el)
```

### Run actual code with outputs


```python
import numpy as np

x = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
])
x
```




    array([[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]])




```python
import pandas as pd

# NOTE: need to set this option, or table renders
# come out wonky when coverting to markdown
pd.set_option('display.notebook_repr_html', False)

rows = [
    {
        'column_1': 1,
        'column_2': 'yippee',
        'column_3': {'test': 10}
    },
    {
        'column_1': 2,
        'column_2': 'nope',
        'column_3': {'working?': 'maybe...'} 
    },
] 
df = pd.DataFrame(rows)
df
```




       column_1 column_2                  column_3
    0         1   yippee              {'test': 10}
    1         2     nope  {'working?': 'maybe...'}




```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

x = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
y = x ** 2

ax.plot(x, y, color='green')
ax.set_title('my plot')
None
```


    
![png](/render_notebook_files/render_notebook_4_0.png)
    


### Latex

This is an inline equation: $f(x) = mx + b$

A fancy inline summation: $\sum^n_i(x_i^2)$

And new line equations $$f(x) = \beta_0 + \beta_1 * x_1 + \beta_2 * x_2$$

# How to get the same functionality

The following steps should be able to be applied to most statically rendered websites

## Step 1: Have `Katex` Support

If you are using a static site generator, such as [hugo](https://gohugo.io/), all of your websites
posts will be some sort of `.md` file. Since most of the notebooks I work with for classes make use 
of `Latex` equations, having this work out of the box is super important for me.

Luckily, `Katex` has awesome `auto-rendering` capabilities that come out of the box. To use these on your
own site, look to the [Auto-render Extension:Usage](https://katex.org/docs/autorender.html#usage) docs. 

Out of the box, rendering is only supported for `$$` delimiters, and will render these on their own line.
So, to get inline latex, you need to add the `$` delimiter option. Below is my custom footer applied to every page:

```html
{{- /* Footer custom content area start */ -}}
{{- /*     Insert any custom code web-analytics, resources, etc. here */ -}}
{{- /* Footer custom content area end */ -}}
<!-- https://katex.org/docs/autorender.html#usage -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" integrity="sha384-n8MVd4RsNIU0tAv4ct0nTaAbDJwPJzDEaqSD1odI+WdtXRGWt2kTvGFasHpSy3SV" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" integrity="sha384-XjKyOOlGwcjNTAIQHIpgOno0Hl1YQqzUOEleOLALmuqehneUG+vnGctmUb0ZY0l8" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" integrity="sha384-+VBxd3r6XgURycqtZ117nYw44OOcIax56Z4dCRWbxyPt0Koah1uHoK0o4+/RRE05" crossorigin="anonymous"></script>
<script>
    document.addEventListener("DOMContentLoaded", function() {
        renderMathInElement(document.body, {
            // customised options
            // • auto-render specific keys, e.g.:
            delimiters: [
                {left: '$$', right: '$$', display: true},
                // render inline
                {left: '$', right: '$', display: false}
            ],
            // • rendering keys, e.g.:
            throwOnError : false
        });
    });
</script>
```

## Step 2: Utilize `jupyter nbconvert`

Once you have a notebook ready to be posted, the [nbcovert](https://pypi.org/project/nbconvert/) package makes it easy
to covert from a `.ipynb` to any format. For this use case, the `markdown` format is what we want:

```bash
jupyter nbconvert --to markdown [notebook].ipynb
```

Once done, you will get a nice `[notebook].md` file, along with 
`[notebook]_files` directory with any static images you generated (such as plots).

Now that you have both of these, only thing necessary to do is copy these over to your
site generator and test them out! 

Some additonal things you may need to do:
- If using `hugo`, you may need to add a post header to the top of the markdown file, like:
    ```md
    ---
    title: "Who needs titles"
    date: 2024-01-25T13:16:30-05:00
    draft: false
    ---
    ```
- To get static file generation working, I needed to move the `[notebook]_files` to my `static`
directory for my site. In addition to this, I also modified the relative link for plots in the `[notebook].md`
file to site links, like so:
    ```md
    **before**
    ![png](notebook_files/notebook_4_0.png)
    **after**
    ![png](/notebook_files/notebook_4_0.png)
    ```


# Voila

You can now use these combined tools to easily render jupyter notebooks to your static site!

Only steps I am thinking of improving is making this into a full blown script, but that is work
for another night
