---
title: JSONata
keywords: jsonata
sidebar: mydoc_sidebar
permalink: index.html
summary: JSON query and transformation language
---

## Introduction

JSONata is a lightweight query and transformation language for JSON data. Inspired by the 'location path' semantics of XPath 3.1, it allows sophisticated queries to be expressed in a compact and intuitive notation.  A rich complement of built in operators and functions is provided for manipulating and combining extracted data, and the results of queries can be formatted into any JSON output structure using familiar JSON object and array syntax. Coupled with the facility to create user defined functions, advanced expressions can be built to tackle any JSON query and transformation task.

<p><iframe width="400" height="300" src="https://www.youtube.com/embed/ZBaK40rtIBM" frameborder="0" allowfullscreen></iframe></p>

Try it out at [http://try.jsonata.org/](http://try.jsonata.org/)

## Install
- `npm install jsonata`

## Usage

In Node.js:

<div class="language-javascript highlighter-rouge"><pre class="highlight"><code><span class="kd">var</span> <span class="nx">jsonata</span> <span class="o">=</span> <span class="nx">require</span><span class="p">(</span><span class="s2">"jsonata"</span><span class="p">);</span>
<span class="kd">var</span> <span class="nx">data</span> <span class="o">=</span> <span class="p">{</span> <span class="s2">"example"</span><span class="p">:</span> <span class="p">[</span> <span class="p">{</span><span class="s2">"value"</span><span class="p">:</span> <span class="mi">4</span><span class="p">},</span> <span class="p">{</span><span class="s2">"value"</span><span class="p">:</span> <span class="mi">7</span><span class="p">},</span> <span class="p">{</span><span class="s2">"value"</span><span class="p">:</span> <span class="mi">13</span><span class="p">}]</span> <span class="p">};</span>
<span class="kd">var</span> <span class="nx">expression</span> <span class="o">=</span> <span class="s2">"$sum(example.value)"</span><span class="p">;</span>
<span class="kd">var</span> <span class="nx">result</span> <span class="o">=</span> <span class="nx">jsonata</span><span class="p">(</span><span class="nx">expression</span><span class="p">).</span><span class="nx">evaluate</span><span class="p">(</span><span class="nx">data</span><span class="p">);</span>  <span class="c1">// returns 24</span>
</code></pre>
</div>

In a browser:

<div class="language-html highlighter-rouge"><pre class="highlight"><code><span class="cp">&lt;!DOCTYPE html&gt;</span>
<span class="nt">&lt;html</span> <span class="na">lang=</span><span class="s">"en"</span><span class="nt">&gt;</span>
<span class="nt">&lt;head&gt;</span>
    <span class="nt">&lt;meta</span> <span class="na">charset=</span><span class="s">"UTF-8"</span><span class="nt">&gt;</span>
    <span class="nt">&lt;title&gt;</span>JSONata test<span class="nt">&lt;/title&gt;</span>
    <span class="nt">&lt;script </span><span class="na">src=</span><span class="s">"lib/jsonata.js"</span><span class="nt">&gt;&lt;/script&gt;</span>
    <span class="nt">&lt;script&gt;</span>
        <span class="kd">function</span> <span class="nx">greeting</span><span class="p">()</span> <span class="p">{</span>
            <span class="kd">var</span> <span class="nx">json</span> <span class="o">=</span> <span class="nx">JSON</span><span class="p">.</span><span class="nx">parse</span><span class="p">(</span><span class="nb">document</span><span class="p">.</span><span class="nx">getElementById</span><span class="p">(</span><span class="s1">'json'</span><span class="p">).</span><span class="nx">value</span><span class="p">);</span>
            <span class="kd">var</span> <span class="nx">result</span> <span class="o">=</span> <span class="nx">jsonata</span><span class="p">(</span><span class="s1">'"Hello, " &amp; name'</span><span class="p">).</span><span class="nx">evaluate</span><span class="p">(</span><span class="nx">json</span><span class="p">);</span>
            <span class="nb">document</span><span class="p">.</span><span class="nx">getElementById</span><span class="p">(</span><span class="s1">'greeting'</span><span class="p">).</span><span class="nx">innerHTML</span> <span class="o">=</span> <span class="nx">result</span><span class="p">;</span>
        <span class="p">}</span>
    <span class="nt">&lt;/script&gt;</span>
<span class="nt">&lt;/head&gt;</span>
<span class="nt">&lt;body&gt;</span>
    <span class="nt">&lt;textarea</span> <span class="na">id=</span><span class="s">"json"</span><span class="nt">&gt;</span>{ "name": "Wilbur" }<span class="nt">&lt;/textarea&gt;</span>
    <span class="nt">&lt;button</span> <span class="na">onclick=</span><span class="s">"greeting()"</span><span class="nt">&gt;</span>Click me<span class="nt">&lt;/button&gt;</span>
    <span class="nt">&lt;p</span> <span class="na">id=</span><span class="s">"greeting"</span><span class="nt">&gt;&lt;/p&gt;</span>
<span class="nt">&lt;/body&gt;</span>
<span class="nt">&lt;/html&gt;</span>
</code></pre>
</div>

## Developers

If you want to run the latest code from Git, here's how to get started:

1. Clone the code:

        git clone https://github.com/jsonata-js/jsonata.git
        cd jsonata

2. Install the development dependencies (there are no runtime dependencies):

        npm install

3. Run the tests

        npm t


## Errors

If an expression throws an error, e.g. syntax error or a runtime error (type error), then the object thrown has a consistent structure containing the column number of the error, the token that caused the error, and any other relevant information, including a meaningful message string.

For example:

```
{
  "position": 16,
  "token": "}",
  "value": "]",
  "message": "Syntax error: expected ']' got '}'"
}
```
