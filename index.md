---
title: JSONata
keywords: jsonata
tags: [getting_started]
sidebar: mydoc_sidebar
permalink: index.html
summary: JSON query and transformation language
---

## Introduction
The primary purpose of this language is to extract values from JSON documents, with the
additional capabilities to combine these values using a set of basic functions
and operators, and also the ability to format the output into any arbitrary JSON structure.

## Install
- `npm install jsonata`

## Usage
In node.js (works in v0.10 and later):
<div class="highlighter-rouge"><pre class="highlight"><code>var jsonata = require("jsonata");
var data = { "example": [ {"value": 4}, {"value": 7}, {"value": 13}] };
var expression = "$sum(example.value)";
var result = jsonata(expression).evaluate(data);  // returns 24
</code></pre>
</div>

In a browser (works in latest Chrome, Firefox, Safari):
<div class="highlighter-rouge"><pre class="highlight"><code><span class="cp">&lt;!DOCTYPE html&gt;</span>
<span class="nt">&lt;html</span> <span class="na">lang=</span><span class="s">"en"</span><span class="nt">&gt;</span>
<span class="nt">&lt;head&gt;</span>
    <span class="nt">&lt;meta</span> <span class="na">charset=</span><span class="s">"UTF-8"</span><span class="nt">&gt;</span>
    <span class="nt">&lt;title&gt;</span>JSONata test<span class="nt">&lt;/title&gt;</span>
    <span class="nt">&lt;script </span><span class="na">src=</span><span class="s">"lib/jsonata.js"</span><span class="nt">&gt;&lt;/script&gt;</span>
<span class="nt">&lt;/head&gt;</span>
<span class="nt">&lt;body&gt;</span>
<span class="nt">&lt;button</span> <span class="na">onclick=</span><span class="s">"alert(jsonata('[1..10]').evaluate())"</span><span class="nt">&gt;</span>Click me<span class="nt">&lt;/button&gt;</span>
<span class="nt">&lt;/body&gt;</span>
<span class="nt">&lt;/html&gt;</span>
</code></pre>
</div>

## Developers
If you want to run the latest code from git, here's how to get started:

2. Clone the code:

        git clone https://github.com/jsonata-js/jsonata.git
        cd jsonata

3. Install the development dependencies (there are no runtime dependencies):

        npm install

4. Run the tests

        npm t


## Errors

If an expression throws an error, e.g. syntax error or a runtime error (type error), then the object thrown
has a consistent structure containing the column number of the error, the token that caused the error,
and any other relevant information, including a meaningful message string.

For example:

`{ "position": 16, "token": "}", "value": "]", "message": "Syntax error: expected ']' got '}' at column 16" }`
