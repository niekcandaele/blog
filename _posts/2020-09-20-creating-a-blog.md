---
tags: jekyll, meta
---

# Research

What platform should we use?

- Ghost

Is nice but requires running a server somewhere to render pages. I would prefer is being fully static which makes hosting a lot simpler.

- VuePress

Used it before for docs. Is nice and easy to work with. Outputs simple static pages, renders from plain markdown. Solid option, but would like to explore others

- Gatsby

https://github.com/gatsbyjs/gatsby-starter-blog
Looks nice, but too complicated. I just want to write some stuff and have it appear online.

- Medium

Meh, not a fan. You do not own your own content. They have the registration-wall now, I despise having my content subject to that.

- Jekyll

Easy hosting with GH pages (However, Vercel/Netlify/... probably provide better UX)
Many themes available
Pretty 'low tech'
Cool theme: https://github.com/cotes2020/jekyll-theme-chirpy/

- Hugo

Similar to Jekyll

## Important factors

- Easy (free) to host
- Content is markdown (No vendor lock in)
- Content is managed in Git
- Automate-able. I don't want to FTP over files or something. I want to push to git and have it deployed

Let's give Jekyll a whirl!

# Setting up Jekyll

```
gem install jekyll bundler
```
