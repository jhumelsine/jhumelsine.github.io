---
title: Core Concepts
description: Links and resources for getting started writing articles using GitHub Pages
date: 2020-09-24 20:30:00
---

# Github Pages Core Concepts

* [Mastering Markdown](https://guides.github.com/features/mastering-markdown/) (or [PDF](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf))
* [How to Embed YouTube](https://thisisa.blog/how-to-embed-media-github-pages)
* [Github Pages Themes](https://pages.github.com/themes/)
* [_config.yml](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll) (Jekyll configuration)
* [Front matter](https://jekyllrb.com/docs/front-matter/)

### Images
* Store page content as markdown files in `/`
* Store image files in `/assets/images/`

![](/assets/images/art.jpg)

### Source Code

```cs
// source code
private void Form1_DragDrop(object sender, DragEventArgs e)
{
    foreach (string path in e.Data.GetData(DataFormats.FileDrop))
    {
        Console.WriteLine(path);
    }
}
```

### YouTube
<iframe src="https://www.youtube.com/embed/1ygdAiDxKfI" 
    width="560" 
    height="315"
    frameborder="0" 
    allowfullscreen>
</iframe>

### Tables

 Name               | Charge | Conductivity (E-4) | C0 (mM)      | CL (mM)      
--------------------|--------|--------------------|--------------|--------------
 K                  | +1     | 73.5               | 145          | 2.8098265   
 Na                 | +1     | 50.11              | 13           | 144.9794365 
 Mg                 | +2     | 53.06              | 1            | 1.9998212   
 Ca                 | +2     | 59.5               | 0            | 0.9999109   
 HEPES              | -1     | 22.05              | 5            | 4.9990023   
 Gluconate          | -1     | 24.255             | 145          | 0           
 Cl                 | -1     | 76.31              | 10           | 148.789725