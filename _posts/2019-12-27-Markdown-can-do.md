---
layout: post
title: What Markdown can do
date: 2019-12-27
tags: HTML CSS Markdown
category: Markdown
toc: 
  - name: It can embed HTML tags
  - name: Display programm code
  - name: Adding DISQUS comments
  - name: Adding table of contents
  - name: customized blockquotes
  - name: Redirecting to another page
  - name: Display tables Bootstrap Tables
  - name: Video
  - name: Audio
---

Markdown was designed for the web, and also easy be read as plaintext.
Basically, we need a parser to transfer markdown into HTML.
There are many parsers for markdown, like kramdown, Python Markdown, etc. So, they might work differently.

## It can embed HTML tags

#### Direct link:
 <a href="https://en.wikipedia.org/">wikipedia</a> 

#### Make a list
<ul>
    <li>Milk</li>
    <li>Bread</li>
    <li>Yogurt</li>
    <li>Nappy</li>
</ul>

<hr>

#### We can Quote:
<blockquote>
    We do not grow absolutely, chronologically. We grow sometimes in one dimension, and not in another, unevenly. We grow partially. We are relative. We are mature in one realm, childish in another.
    —Anais Nin
</blockquote>

#### Support Images:

{% include figure.html path="assets/img/7.jpg" class="img-fluid rounded z-depth-1" width="80%" %}

responsive format:
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/9.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/7.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Display programm code

#### You have to do is wrap your code in markdown code tags:

````markdown
```c++
code code code
```
````

```c++
int main(int argc, char const \*argv[])
{
    string myString;

    cout << "input a string: ";
    getline(cin, myString);
    int length = myString.length();

    char charArray = new char * [length];

    charArray = myString;
    for(int i = 0; i < length; ++i){
        cout << charArray[i] << " ";
    }

    return 0;
}
```

#### Display line number

{% highlight c++ linenos %}

int main(int argc, char const \*argv[])
{
    string myString;

    cout << "input a string: ";
    getline(cin, myString);
    int length = myString.length();

    char charArray = new char * [length];

    charArray = myString;
    for(int i = 0; i < length; ++i){
        cout << charArray[i] << " ";
    }

    return 0;
}

{% endhighlight %}

#### Display liquid template code too
Using `{ % raw % }` and `{ % endraw % }` to wrap liquid code and then, the liquid code can display:

{% raw %}
{% highlight c++ linenos %}  <br/> code code code <br/> {% endhighlight %}
{% endraw %}

#### Support Mathjax

You just need to surround your math expression with `$$`, like `$$ E = mc^2 $$`. Example: $$ E = mc^2 $$.

You can also use `\begin{equation}...\end{equation}` instead of `$$` for display mode math.
MathJax will automatically number equations:

\begin{equation}
\label{eq:cauchy-schwarz}
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
\end{equation}


## Adding DISQUS comments

Turn `disqus_comments: true` on at the Front Matter.

## Adding table of contents

To add a table of contents to a post, add
```yml
toc:
  beginning: true
```
or 
```yml
toc:
  sidebar: left
```
to the front matter of the post. The table of contents will be automatically generated from the headings in the post.


## customized blockquotes

Like below:
```markdown
> ##### WARNING
>
> This is a warning, and thus should
> be used when you want to warn the user
{: .block-warning }
```

> ##### WARNING
>
> This is a warning, and thus should
> be used when you want to warn the user
{: .block-warning }

```markdown
> ##### DANGER
>
> This is a danger zone, and thus should
> be used carefully
{: .block-danger }
```

> ##### DANGER
>
> This is a danger zone, and thus should
> be used carefully
{: .block-danger }

## Redirecting to another page

Add below in the front matter.

```
redirect: /assets/pdf/example_pdf.pdf
```

## Display tables Bootstrap Tables
Using markdown to display tables is easy. Just use the following syntax:

```markdown
| Left aligned | Center aligned | Right aligned |
| :----------- | :------------: | ------------: |
| Left 1       | center 1       | right 1       |
| Left 2       | center 2       | right 2       |
| Left 3       | center 3       | right 3       |
```

That will generate:

| Left aligned | Center aligned | Right aligned |
| :----------- | :------------: | ------------: |
| Left 1       | center 1       | right 1       |
| Left 2       | center 2       | right 2       |
| Left 3       | center 3       | right 3       |

<p></p>

## Video

#### It supports local video files.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/pexels-engin-akyurt-6069112-960x540-30fps.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="assets/video/pexels-engin-akyurt-6069112-960x540-30fps.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    A simple, elegant caption looks good between video rows, after each row, or doesn't have to be there at all.
</div>

#### It does also support embedding videos from different sources.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="https://www.youtube.com/embed/jNQXAC9IVRw" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include video.html path="https://player.vimeo.com/video/524933864?h=1ac4fd9fb4&title=0&byline=0&portrait=0" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Audio
It supports local and external audio files.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include audio.html path="assets/audio/epicaly-short-113909.mp3" controls=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include audio.html path="https://cdn.pixabay.com/download/audio/2022/06/25/audio_69a61cd6d6.mp3" controls=true %}
    </div>
</div>
<div class="caption">
    A simple, elegant caption looks good between video rows, after each row, or doesn't have to be there at all.
</div>

