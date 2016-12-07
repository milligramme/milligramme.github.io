+++
title = "Markdown Parser を比較"
date = "2013-10-30T00:00:00+09:00"
tags = ["markdown", "ruby"]
+++

### redcarpetとrdiscountとkramdown

- [vmg/redcarpet](https://github.com/vmg/redcarpet)
- [davidfstr/rdiscount](https://github.com/davidfstr/rdiscount)
- [gettalong/kramdown](https://github.com/gettalong/kramdown)

サンプル生成

<script src="https://gist.github.com/milligramme/bfd5556e747141ef6259.js"></script>

### :redcarpet

```html
  <h1>Hello World</h1>

  <p>こんにちは <em>世界</em> に **さよなら** 。 <a href="http://www.example.com/">http://www.example.com/</a></p>

  <p><a href="http://www.example.com/">たとえば</a></p>

  <blockquote>
  <p>リスト</p>
  </blockquote>

  <ol>
  <li>number</li>
  <li>number</li>
  <li>number</li>
  <li>number</li>
  </ol>

  <blockquote>
  <p>多段リスト</p>
  </blockquote>

  <ul>
  <li>list</li>
  <li>list

  <ul>
  <li>list</li>
  <li>list</li>
  </ul></li>
  </ul>

  <h4>4スペ コード</h4>

  <pre><code>def foo
    @bar = &quot;bar&quot;
    @coo = 1234
  end
  </code></pre>

  <h4>``` コード</h4>

  <pre><code class="ruby">def foo
    @bar = &quot;bar&quot;
    @coo = 1234
  end
  </code></pre>

  <h4>``` コード</h4>

  <pre><code class="ruby">def foo
    @bar = &quot;bar&quot;
    @coo = 1234
  end
  </code></pre>
```

---

### :rdiscount

```html

  <h1>Hello World</h1>

  <p>こんにちは <em>世界</em> に **さよなら** 。 <a href="http://www.example.com/">http://www.example.com/</a></p>

  <p><a href="http://www.example.com/">たとえば</a></p>

  <blockquote><p>リスト</p></blockquote>

  <ol>
  <li>number</li>
  <li>number</li>
  <li>number</li>
  <li>number</li>
  </ol>


  <blockquote><p>多段リスト</p></blockquote>

  <ul>
  <li>list</li>
  <li>list

  <ul>
  <li>list</li>
  <li>list</li>
  </ul>
  </li>
  </ul>


  <h4>4スペ コード</h4>

  <pre><code>def foo
    @bar = "bar"
    @coo = 1234
  end
  </code></pre>

  <h4>``` コード</h4>

  <pre><code class="ruby">def foo
    @bar = "bar"
    @coo = 1234
  end
  </code></pre>

  <h4>``` コード</h4>

  <pre><code class="ruby">def foo
    @bar = "bar"
    @coo = 1234
  end
  </code></pre>
```

---

### :kramdown

```html
  <h1 id="hello-world">Hello World</h1>
  <p>こんにちは <em>世界</em> に **さよなら** 。 http://www.example.com/</p>

  <p><a href="http://www.example.com/">たとえば</a></p>

  <blockquote>
    <p>リスト</p>
  </blockquote>

  <ol>
    <li>number</li>
    <li>number</li>
    <li>number</li>
    <li>number</li>
  </ol>

  <blockquote>
    <p>多段リスト</p>
  </blockquote>

  <ul>
    <li>list</li>
    <li>list
      <ul>
        <li>list</li>
        <li>list</li>
      </ul>
    </li>
  </ul>

  <h4 id="section">4スペ コード</h4>

  <pre><code>def foo
    @bar = "bar"
    @coo = 1234
  end
  </code></pre>

  <h4 id="section-1">``` コード</h4>

  <p><code>ruby
  def foo
    @bar = "bar"
    @coo = 1234
  end
  </code></p>

  <h4 id="section-2">``` コード</h4>

  <div><div class="CodeRay">
    <div class="code"><pre><span class="line-numbers"><a href="#n1" name="n1">1</a></span><span style="color:#080;font-weight:bold">def</span> <span style="color:#06B;font-weight:bold">foo</span>
  <span class="line-numbers"><a href="#n2" name="n2">2</a></span>  <span style="color:#33B">@bar</span> = <span style="background-color:hsla(0,100%,50%,0.05)"><span style="color:#710">&quot;</span><span style="color:#D20">bar</span><span style="color:#710">&quot;</span></span>
  <span class="line-numbers"><a href="#n3" name="n3">3</a></span>  <span style="color:#33B">@coo</span> = <span style="color:#00D">1234</span>
  <span class="line-numbers"><a href="#n4" name="n4">4</a></span><span style="color:#080;font-weight:bold">end</span>
  </pre></div>
  </div>
  </div>
```

---
