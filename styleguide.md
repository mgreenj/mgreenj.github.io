---
title: Styleguide
layout: page
permalink: /styleguide/
---


### devlopr - Styleguide

<hr />

 <img src="/assets/img/styleguide.png" class="img-fluid">

<p> Lets try the different text styles  <b> Bold </b> , <strong> Strong </strong>, <em> Emphasis </em>, <i> Italic </i> </p>


<p> Now, lets try different heading styles : </p>

<h1> Hello in h1 ! </h1>
<h2> Hello in h2 ! </h2>
<h3> Hello in h3 ! </h3>
<h4> Hello in h4 ! </h4>
<h5> Hello in h5 ! </h5>
<h6> Hello in h6 ! </h6>

<hr />
<p> Unordered List </p>

<ul>
<li> List Item 1 </li>
<li> List Item 2 </li>
<li> List Item 3 </li>
<li> List Item 4 </li>
<li> List Item 5 </li>
</ul>

<p> Ordered List </p>
<ol>
<li> List Item 1 </li>
<li> List Item 2 </li>
<li> List Item 3 </li>
<li> List Item 4 </li>
<li> List Item 5 </li>
</ol>

<blockquote>
<p>This is a Block Quote,  It can Expand Multiple Lines </p>

</blockquote>

<p>You can use the mark tag to <mark>highlight</mark> text. </p>

<p><del> This line of text is meant to be deleted text </del> </p>

<p><u>This line of text will render as underlined</u></p>
<p><small>This line of text is meant to be treated as fine print.</small></p>
<p><strong>This line rendered as bold text.</strong></p>
<p><em>This line rendered as italicized text.</em></p>
<p><abbr title="attribute">attr</abbr></p>
<p><abbr title="HyperText Markup Language" class="initialism">HTML</abbr></p>

<hr />
<div class="responsive-table">
<table>
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
          <th scope="col">Heading</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">1</th>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
        </tr>
        <tr>
          <th scope="row">2</th>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
        </tr>
        <tr>
          <th scope="row">3</th>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
          <td>Cell</td>
        </tr>
      </tbody>
    </table>
    </div>

<hr />


<h3> Twitter Embed </h3>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Looking Forward to reading and posting a lot of security content.</p>&mdash; Sujay Kundu (@SujayKundu777) <a href="https://twitter.com/tallB0y3883/status/1769480868022636768?ref_src=twsrc%5Etfw">June 29, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<hr />


<h3>YouTube Responsive Embed</h3>

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=yCd3CzGSte8?rel=0&amp;controls=0&amp;showinfo=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<hr />

<h3 id="codepen-embed">CodePen Embed</h3>

<p data-height="265" data-theme-id="light" data-slug-hash="YWvpRo" data-default-tab="css,result" data-user="kharrop" data-embed-version="2" data-pen-title="Referral Form" class="codepen"></p>
<script async="" src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<hr />

<h3 id="syntax-highlighting">Syntax Highlighting</h3>

<figure class="highlight"><pre><code class="language-js" data-lang="js"><span class="s1">'use strict'</span><span class="p">;</span>
<span class="kd">var</span> <span class="nx">markdown</span> <span class="o">=</span> <span class="nx">require</span><span class="p">(</span><span class="s1">'markdown'</span><span class="p">).</span><span class="nx">markdown</span><span class="p">;</span>
<span class="kd">function</span> <span class="nx">Editor</span><span class="p">(</span><span class="nx">input</span><span class="p">,</span> <span class="nx">preview</span><span class="p">)</span> <span class="p">{</span>
  <span class="k">this</span><span class="p">.</span><span class="nx">update</span> <span class="o">=</span> <span class="kd">function</span><span class="p">()</span> <span class="p">{</span>
    <span class="nx">preview</span><span class="p">.</span><span class="nx">innerHTML</span> <span class="o">=</span> <span class="nx">markdown</span><span class="p">.</span><span class="nx">toHTML</span><span class="p">(</span><span class="nx">input</span><span class="p">.</span><span class="nx">value</span><span class="p">);</span>
  <span class="p">};</span>
  <span class="nx">input</span><span class="p">.</span><span class="nx">editor</span> <span class="o">=</span> <span class="k">this</span><span class="p">;</span>
  <span class="k">this</span><span class="p">.</span><span class="nx">update</span><span class="p">();</span>
<span class="p">}</span></code></pre></figure>

<p>You can add inline code just like this, E.g. <code class="highlighter-rouge">.code { color: #fff; }</code></p>

<figure class="highlight"><pre><code class="language-css" data-lang="css"><span class="nt">pre</span> <span class="p">{</span>
  <span class="nl">background-color</span><span class="p">:</span> <span class="m">#f4f4f4</span><span class="p">;</span>
  <span class="nl">max-width</span><span class="p">:</span> <span class="m">100%</span><span class="p">;</span>
  <span class="nl">overflow</span><span class="p">:</span> <span class="nb">auto</span><span class="p">;</span>
<span class="p">}</span></code></pre></figure>

<hr />

<h3 id="github-gist-embed">GitHub gist Embed</h3>

<script src="https://gist.github.com/ahmadajmi/dbb4f713317721668bcbc39420562afc.js"></script>

<hr />

<h3 id="input-style">Input Style</h3>

<p><input type="text" placeholder="I'm an input field!" /></p>

<hr />


