---
src       : src/plfa/Negation.lagda
title     : "Negation: Negation, with intuitionistic and classical logic"
layout    : page
prev      : /Connectives/
permalink : /Negation/
next      : /Quantifiers/
---

<pre class="Agda">{% raw %}<a id="189" class="Keyword">module</a> <a id="196" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}" class="Module">plfa.Negation</a> <a id="210" class="Keyword">where</a>{% endraw %}</pre>

This chapter introduces negation, and discusses intuitionistic
and classical logic.

## Imports

<pre class="Agda">{% raw %}<a id="338" class="Keyword">open</a> <a id="343" class="Keyword">import</a> <a id="350" href="https://agda.github.io/agda-stdlib/Relation.Binary.PropositionalEquality.html" class="Module">Relation.Binary.PropositionalEquality</a> <a id="388" class="Keyword">using</a> <a id="394" class="Symbol">(</a><a id="395" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">_≡_</a><a id="398" class="Symbol">;</a> <a id="400" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#140" class="InductiveConstructor">refl</a><a id="404" class="Symbol">)</a>
<a id="406" class="Keyword">open</a> <a id="411" class="Keyword">import</a> <a id="418" href="https://agda.github.io/agda-stdlib/Data.Nat.html" class="Module">Data.Nat</a> <a id="427" class="Keyword">using</a> <a id="433" class="Symbol">(</a><a id="434" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="435" class="Symbol">;</a> <a id="437" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a><a id="441" class="Symbol">;</a> <a id="443" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a><a id="446" class="Symbol">)</a>
<a id="448" class="Keyword">open</a> <a id="453" class="Keyword">import</a> <a id="460" href="https://agda.github.io/agda-stdlib/Data.Empty.html" class="Module">Data.Empty</a> <a id="471" class="Keyword">using</a> <a id="477" class="Symbol">(</a><a id="478" href="https://agda.github.io/agda-stdlib/Data.Empty.html#243" class="Datatype">⊥</a><a id="479" class="Symbol">;</a> <a id="481" href="https://agda.github.io/agda-stdlib/Data.Empty.html#360" class="Function">⊥-elim</a><a id="487" class="Symbol">)</a>
<a id="489" class="Keyword">open</a> <a id="494" class="Keyword">import</a> <a id="501" href="https://agda.github.io/agda-stdlib/Data.Sum.html" class="Module">Data.Sum</a> <a id="510" class="Keyword">using</a> <a id="516" class="Symbol">(</a><a id="517" href="https://agda.github.io/agda-stdlib/Data.Sum.Base.html#419" class="Datatype Operator">_⊎_</a><a id="520" class="Symbol">;</a> <a id="522" href="https://agda.github.io/agda-stdlib/Data.Sum.Base.html#475" class="InductiveConstructor">inj₁</a><a id="526" class="Symbol">;</a> <a id="528" href="https://agda.github.io/agda-stdlib/Data.Sum.Base.html#500" class="InductiveConstructor">inj₂</a><a id="532" class="Symbol">)</a>
<a id="534" class="Keyword">open</a> <a id="539" class="Keyword">import</a> <a id="546" href="https://agda.github.io/agda-stdlib/Data.Product.html" class="Module">Data.Product</a> <a id="559" class="Keyword">using</a> <a id="565" class="Symbol">(</a><a id="566" href="https://agda.github.io/agda-stdlib/Data.Product.html#1353" class="Function Operator">_×_</a><a id="569" class="Symbol">;</a> <a id="571" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Sigma.html#155" class="Field">proj₁</a><a id="576" class="Symbol">;</a> <a id="578" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Sigma.html#167" class="Field">proj₂</a><a id="583" class="Symbol">)</a> <a id="585" class="Keyword">renaming</a> <a id="594" class="Symbol">(</a><a id="595" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Sigma.html#139" class="InductiveConstructor Operator">_,_</a> <a id="599" class="Symbol">to</a> <a id="602" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Sigma.html#139" class="InductiveConstructor Operator">⟨_,_⟩</a><a id="607" class="Symbol">)</a>
<a id="609" class="Keyword">open</a> <a id="614" class="Keyword">import</a> <a id="621" href="https://agda.github.io/agda-stdlib/Function.html" class="Module">Function</a> <a id="630" class="Keyword">using</a> <a id="636" class="Symbol">(</a><a id="637" href="https://agda.github.io/agda-stdlib/Function.html#769" class="Function Operator">_∘_</a><a id="640" class="Symbol">)</a>
<a id="642" class="Keyword">open</a> <a id="647" class="Keyword">import</a> <a id="654" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}" class="Module">plfa.Isomorphism</a> <a id="671" class="Keyword">using</a> <a id="677" class="Symbol">(</a><a id="678" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#4104" class="Record Operator">_≃_</a><a id="681" class="Symbol">;</a> <a id="683" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#6802" class="Function">≃-sym</a><a id="688" class="Symbol">;</a> <a id="690" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#7143" class="Function">≃-trans</a><a id="697" class="Symbol">;</a> <a id="699" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Isomorphism.md %}{% raw %}#9013" class="Record Operator">_≲_</a><a id="702" class="Symbol">)</a>{% endraw %}</pre>


## Negation

Given a proposition `A`, the negation `¬ A` holds if `A` cannot hold.
We formalise this idea by declaring negation to be the same
as implication of false.
<pre class="Agda">{% raw %}<a id="¬_"></a><a id="898" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬_</a> <a id="901" class="Symbol">:</a> <a id="903" class="PrimitiveType">Set</a> <a id="907" class="Symbol">→</a> <a id="909" class="PrimitiveType">Set</a>
<a id="913" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="915" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#915" class="Bound">A</a> <a id="917" class="Symbol">=</a> <a id="919" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#915" class="Bound">A</a> <a id="921" class="Symbol">→</a> <a id="923" href="https://agda.github.io/agda-stdlib/Data.Empty.html#243" class="Datatype">⊥</a>{% endraw %}</pre>
This is a form of _proof by contradiction_: if assuming `A` leads
to the conclusion `⊥` (a contradiction), then we must have `¬ A`.

Evidence that `¬ A` holds is of the form

    λ{ x → N }

where `N` is a term of type `⊥` containing as a free variable `x` of type `A`.
In other words, evidence that `¬ A` holds is a function that converts evidence
that `A` holds into evidence that `⊥` holds.

Given evidence that both `¬ A` and `A` hold, we can conclude that `⊥` holds.
In other words, if both `¬ A` and `A` hold, then we have a contradiction.
<pre class="Agda">{% raw %}<a id="¬-elim"></a><a id="1495" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1495" class="Function">¬-elim</a> <a id="1502" class="Symbol">:</a> <a id="1504" class="Symbol">∀</a> <a id="1506" class="Symbol">{</a><a id="1507" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1507" class="Bound">A</a> <a id="1509" class="Symbol">:</a> <a id="1511" class="PrimitiveType">Set</a><a id="1514" class="Symbol">}</a>
  <a id="1518" class="Symbol">→</a> <a id="1520" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="1522" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1507" class="Bound">A</a>
  <a id="1526" class="Symbol">→</a> <a id="1528" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1507" class="Bound">A</a>
    <a id="1534" class="Comment">---</a>
  <a id="1540" class="Symbol">→</a> <a id="1542" href="https://agda.github.io/agda-stdlib/Data.Empty.html#243" class="Datatype">⊥</a>
<a id="1544" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1495" class="Function">¬-elim</a> <a id="1551" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1551" class="Bound">¬x</a> <a id="1554" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1554" class="Bound">x</a> <a id="1556" class="Symbol">=</a> <a id="1558" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1551" class="Bound">¬x</a> <a id="1561" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#1554" class="Bound">x</a>{% endraw %}</pre>
Here we write `¬x` for evidence of `¬ A` and `x` for evidence of `A`.  This
means that `¬x` must be a function of type `A → ⊥`, and hence the application
`¬x x` must be of type `⊥`.  Note that this rule is just a special case of `→-elim`.

We set the precedence of negation so that it binds more tightly
than disjunction and conjunction, but less tightly than anything else.
<pre class="Agda">{% raw %}<a id="1962" class="Keyword">infix</a> <a id="1968" class="Number">3</a> <a id="1970" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬_</a>{% endraw %}</pre>
Thus, `¬ A × ¬ B` parses as `(¬ A) × (¬ B)` and `¬ m ≡ n` as `¬ (m ≡ n)`.

In _classical_ logic, we have that `A` is equivalent to `¬ ¬ A`.
As we discuss below, in Agda we use _intuitionistic_ logic, where
we have only half of this equivalence, namely that `A` implies `¬ ¬ A`.
<pre class="Agda">{% raw %}<a id="¬¬-intro"></a><a id="2275" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2275" class="Function">¬¬-intro</a> <a id="2284" class="Symbol">:</a> <a id="2286" class="Symbol">∀</a> <a id="2288" class="Symbol">{</a><a id="2289" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2289" class="Bound">A</a> <a id="2291" class="Symbol">:</a> <a id="2293" class="PrimitiveType">Set</a><a id="2296" class="Symbol">}</a>
  <a id="2300" class="Symbol">→</a> <a id="2302" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2289" class="Bound">A</a>
    <a id="2308" class="Comment">-----</a>
  <a id="2316" class="Symbol">→</a> <a id="2318" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="2320" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="2322" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2289" class="Bound">A</a>
<a id="2324" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2275" class="Function">¬¬-intro</a> <a id="2333" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2333" class="Bound">x</a> <a id="2335" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2335" class="Bound">¬x</a> <a id="2338" class="Symbol">=</a> <a id="2340" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2335" class="Bound">¬x</a> <a id="2343" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2333" class="Bound">x</a>{% endraw %}</pre>
Let `x` be evidence of `A`. We show that assuming
`¬ A` leads to a contradiction, and hence `¬ ¬ A` must hold.
Let `¬x` be evidence of `¬ A`.  Then from `A` and `¬ A`
we have a contradiction, evidenced by `¬x x`.  Hence, we have
shown `¬ ¬ A`.

We cannot show that `¬ ¬ A` implies `A`, but we can show that
`¬ ¬ ¬ A` implies `¬ A`.
<pre class="Agda">{% raw %}<a id="¬¬¬-elim"></a><a id="2701" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2701" class="Function">¬¬¬-elim</a> <a id="2710" class="Symbol">:</a> <a id="2712" class="Symbol">∀</a> <a id="2714" class="Symbol">{</a><a id="2715" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2715" class="Bound">A</a> <a id="2717" class="Symbol">:</a> <a id="2719" class="PrimitiveType">Set</a><a id="2722" class="Symbol">}</a>
  <a id="2726" class="Symbol">→</a> <a id="2728" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="2730" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="2732" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="2734" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2715" class="Bound">A</a>
    <a id="2740" class="Comment">-------</a>
  <a id="2750" class="Symbol">→</a> <a id="2752" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="2754" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2715" class="Bound">A</a>
<a id="2756" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2701" class="Function">¬¬¬-elim</a> <a id="2765" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2765" class="Bound">¬¬¬x</a> <a id="2770" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2770" class="Bound">x</a> <a id="2772" class="Symbol">=</a> <a id="2774" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2765" class="Bound">¬¬¬x</a> <a id="2779" class="Symbol">(</a><a id="2780" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2275" class="Function">¬¬-intro</a> <a id="2789" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#2770" class="Bound">x</a><a id="2790" class="Symbol">)</a>{% endraw %}</pre>
Let `¬¬¬x` be evidence of `¬ ¬ ¬ A`. We will show that assuming
`A` leads to a contradiction, and hence `¬ A` must hold.
Let `x` be evidence of `A`. Then by the previous result, we
can conclude `¬ ¬ A`, evidenced by `¬¬-intro x`.  Then from
`¬ ¬ ¬ A` and `¬ ¬ A` we have a contradiction, evidenced by
`¬¬¬x (¬¬-intro x)`.  Hence we have shown `¬ A`.

Another law of logic is _contraposition_,
stating that if `A` implies `B`, then `¬ B` implies `¬ A`.
<pre class="Agda">{% raw %}<a id="contraposition"></a><a id="3268" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3268" class="Function">contraposition</a> <a id="3283" class="Symbol">:</a> <a id="3285" class="Symbol">∀</a> <a id="3287" class="Symbol">{</a><a id="3288" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3288" class="Bound">A</a> <a id="3290" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3290" class="Bound">B</a> <a id="3292" class="Symbol">:</a> <a id="3294" class="PrimitiveType">Set</a><a id="3297" class="Symbol">}</a>
  <a id="3301" class="Symbol">→</a> <a id="3303" class="Symbol">(</a><a id="3304" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3288" class="Bound">A</a> <a id="3306" class="Symbol">→</a> <a id="3308" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3290" class="Bound">B</a><a id="3309" class="Symbol">)</a>
    <a id="3315" class="Comment">-----------</a>
  <a id="3329" class="Symbol">→</a> <a id="3331" class="Symbol">(</a><a id="3332" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="3334" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3290" class="Bound">B</a> <a id="3336" class="Symbol">→</a> <a id="3338" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="3340" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3288" class="Bound">A</a><a id="3341" class="Symbol">)</a>
<a id="3343" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3268" class="Function">contraposition</a> <a id="3358" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3358" class="Bound">f</a> <a id="3360" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3360" class="Bound">¬y</a> <a id="3363" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3363" class="Bound">x</a> <a id="3365" class="Symbol">=</a> <a id="3367" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3360" class="Bound">¬y</a> <a id="3370" class="Symbol">(</a><a id="3371" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3358" class="Bound">f</a> <a id="3373" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3363" class="Bound">x</a><a id="3374" class="Symbol">)</a>{% endraw %}</pre>
Let `f` be evidence of `A → B` and let `¬y` be evidence of `¬ B`.  We
will show that assuming `A` leads to a contradiction, and hence `¬ A`
must hold. Let `x` be evidence of `A`.  Then from `A → B` and `A` we
may conclude `B`, evidenced by `f x`, and from `B` and `¬ B` we may
conclude `⊥`, evidenced by `¬y (f x)`.  Hence, we have shown `¬ A`.

Using negation, it is straightforward to define inequality.
<pre class="Agda">{% raw %}<a id="_≢_"></a><a id="3806" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3806" class="Function Operator">_≢_</a> <a id="3810" class="Symbol">:</a> <a id="3812" class="Symbol">∀</a> <a id="3814" class="Symbol">{</a><a id="3815" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3815" class="Bound">A</a> <a id="3817" class="Symbol">:</a> <a id="3819" class="PrimitiveType">Set</a><a id="3822" class="Symbol">}</a> <a id="3824" class="Symbol">→</a> <a id="3826" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3815" class="Bound">A</a> <a id="3828" class="Symbol">→</a> <a id="3830" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3815" class="Bound">A</a> <a id="3832" class="Symbol">→</a> <a id="3834" class="PrimitiveType">Set</a>
<a id="3838" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3838" class="Bound">x</a> <a id="3840" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3806" class="Function Operator">≢</a> <a id="3842" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3842" class="Bound">y</a>  <a id="3845" class="Symbol">=</a>  <a id="3848" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="3850" class="Symbol">(</a><a id="3851" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3838" class="Bound">x</a> <a id="3853" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Equality.html#83" class="Datatype Operator">≡</a> <a id="3855" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3842" class="Bound">y</a><a id="3856" class="Symbol">)</a>{% endraw %}</pre>
It is trivial to show distinct numbers are not equal.
<pre class="Agda">{% raw %}<a id="3936" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3936" class="Function">_</a> <a id="3938" class="Symbol">:</a> <a id="3940" class="Number">1</a> <a id="3942" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3806" class="Function Operator">≢</a> <a id="3944" class="Number">2</a>
<a id="3946" class="Symbol">_</a> <a id="3948" class="Symbol">=</a> <a id="3950" class="Symbol">λ()</a>{% endraw %}</pre>
This is our first use of an absurd pattern in a lambda expression.
The type `M ≡ N` is occupied exactly when `M` and `N` simplify to
identical terms. Since `1` and `2` simplify to distinct normal forms,
Agda determines that there is no possible evidence that `1 ≡ 2`.
As a second example, it is also easy to validate
Peano's postulate that zero is not the successor of any number.
<pre class="Agda">{% raw %}<a id="peano"></a><a id="4359" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4359" class="Function">peano</a> <a id="4365" class="Symbol">:</a> <a id="4367" class="Symbol">∀</a> <a id="4369" class="Symbol">{</a><a id="4370" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4370" class="Bound">m</a> <a id="4372" class="Symbol">:</a> <a id="4374" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#97" class="Datatype">ℕ</a><a id="4375" class="Symbol">}</a> <a id="4377" class="Symbol">→</a> <a id="4379" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#115" class="InductiveConstructor">zero</a> <a id="4384" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#3806" class="Function Operator">≢</a> <a id="4386" href="https://agda.github.io/agda-stdlib/Agda.Builtin.Nat.html#128" class="InductiveConstructor">suc</a> <a id="4390" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4370" class="Bound">m</a>
<a id="4392" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4359" class="Function">peano</a> <a id="4398" class="Symbol">=</a> <a id="4400" class="Symbol">λ()</a>{% endraw %}</pre>
The evidence is essentially the same, as the absurd pattern matches
all possible evidence of type `zero ≡ suc m`.

Given the correspondence of implication to exponentiation and
false to the type with no members, we can view negation as
raising to the zero power.  This indeed corresponds to what
we know for arithmetic, where

    0 ^ n  ≡  1,  if n ≡ 0
           ≡  0,  if n ≢ 0

Indeed, there is exactly one proof of `⊥ → ⊥`.
<pre class="Agda">{% raw %}<a id="id"></a><a id="4857" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4857" class="Function">id</a> <a id="4860" class="Symbol">:</a> <a id="4862" href="https://agda.github.io/agda-stdlib/Data.Empty.html#243" class="Datatype">⊥</a> <a id="4864" class="Symbol">→</a> <a id="4866" href="https://agda.github.io/agda-stdlib/Data.Empty.html#243" class="Datatype">⊥</a>
<a id="4868" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4857" class="Function">id</a> <a id="4871" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4871" class="Bound">x</a> <a id="4873" class="Symbol">=</a> <a id="4875" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#4871" class="Bound">x</a>{% endraw %}</pre>
It is easy to see there are no possible values of type `A → ⊥`
unless `A` is equivalent to `⊥`.  We have that `⊥ → A`
always holds, by `⊥-elim`, and hence if `A → ⊥` holds then
`A` must be equivalent to `⊥`, in the sense that each implies
the other.



#### Exercise `<-irreflexive`

Using negation, show that
[strict inequality]({{ site.baseurl }}{% link out/plfa/Relations.md%}#strict-inequality)
is irreflexive, that is, `n < n` holds for no `n`.


#### Exercise `trichotomy`

Show that strict inequality satisfies
[trichotomy]({{ site.baseurl }}{% link out/plfa/Relations.md%}#trichotomy),
that is, for any naturals `m` and `n` exactly one of the following holds:

* `m < n`
* `m ≡ n`
* `m > n`

Here "exactly one" means that one of the three must hold, and each implies the
negation of the other two.


#### Exercise `⊎-dual-×`

Show that conjunction, disjunction, and negation are related by a
version of De Morgan's Law.

    ¬ (A ⊎ B) ≃ (¬ A) × (¬ B)

This result is an easy consequence of something we've proved previously.

Is there also a term of the following type?

    ¬ (A × B) ≃ (¬ A) ⊎ (¬ B)

If so, prove; if not, explain why.


## Intuitive and Classical logic

In Gilbert and Sullivan's _The Gondoliers_, Casilda is told that
as an infant she was married to the heir of the King of Batavia, but
that due to a mix-up no one knows which of two individuals, Marco or
Giuseppe, is the heir.  Alarmed, she wails "Then do you mean to say
that I am married to one of two gondoliers, but it is impossible to
say which?"  To which the response is "Without any doubt of any kind
whatever."

Logic comes in many varieties, and one distinction is between
_classical_ and _intuitionistic_. Intuitionists, concerned
by assumptions made by some logicians about the nature of
infinity, insist upon a constructionist notion of truth.  In
particular, they insist that a proof of `A ⊎ B` must show
_which_ of `A` or `B` holds, and hence they would reject the
claim that Casilda is married to Marco or Giuseppe until one of the
two was identified as her husband.  Perhaps Gilbert and Sullivan
anticipated intuitionism, for their story's outcome is that the heir
turns out to be a third individual, Luiz, with whom Casilda is,
conveniently, already in love.

Intuitionists also reject the law of the excluded middle, which
asserts `A ⊎ ¬ A` for every `A`, since the law gives no clue as to
_which_ of `A` or `¬ A` holds. Heyting formalised a variant of
Hilbert's classical logic that captures the intuitionistic notion of
provability. In particular, the law of the excluded middle is provable
in Hilbert's logic, but not in Heyting's.  Further, if the law of the
excluded middle is added as an axiom to Heyting's logic, then it
becomes equivalent to Hilbert's.  Kolmogorov showed the two logics
were closely related: he gave a double-negation translation, such that
a formula is provable in classical logic if and only if its
translation is provable in intuitionistic logic.

Propositions as Types was first formulated for intuitionistic logic.
It is a perfect fit, because in the intuitionist interpretation the
formula `A ⊎ B` is provable exactly when one exhibits either a proof
of `A` or a proof of `B`, so the type corresponding to disjunction is
a disjoint sum.

(Parts of the above are adopted from "Propositions as Types", Philip Wadler,
_Communications of the ACM_, December 2015.)

## Excluded middle is irrefutable

The law of the excluded middle can be formulated as follows.
<pre class="Agda">{% raw %}<a id="8318" class="Keyword">postulate</a>
  <a id="em"></a><a id="8330" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8330" class="Postulate">em</a> <a id="8333" class="Symbol">:</a> <a id="8335" class="Symbol">∀</a> <a id="8337" class="Symbol">{</a><a id="8338" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8338" class="Bound">A</a> <a id="8340" class="Symbol">:</a> <a id="8342" class="PrimitiveType">Set</a><a id="8345" class="Symbol">}</a> <a id="8347" class="Symbol">→</a> <a id="8349" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8338" class="Bound">A</a> <a id="8351" href="https://agda.github.io/agda-stdlib/Data.Sum.Base.html#419" class="Datatype Operator">⊎</a> <a id="8353" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="8355" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8338" class="Bound">A</a>{% endraw %}</pre>
As we noted, the law of the excluded middle does not hold in
intuitionistic logic.  However, we can show that it is _irrefutable_,
meaning that the negation of its negation is provable (and hence that
its negation is never provable).
<pre class="Agda">{% raw %}<a id="em-irrefutable"></a><a id="8615" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8615" class="Function">em-irrefutable</a> <a id="8630" class="Symbol">:</a> <a id="8632" class="Symbol">∀</a> <a id="8634" class="Symbol">{</a><a id="8635" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8635" class="Bound">A</a> <a id="8637" class="Symbol">:</a> <a id="8639" class="PrimitiveType">Set</a><a id="8642" class="Symbol">}</a> <a id="8644" class="Symbol">→</a> <a id="8646" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="8648" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="8650" class="Symbol">(</a><a id="8651" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8635" class="Bound">A</a> <a id="8653" href="https://agda.github.io/agda-stdlib/Data.Sum.Base.html#419" class="Datatype Operator">⊎</a> <a id="8655" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="8657" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8635" class="Bound">A</a><a id="8658" class="Symbol">)</a>
<a id="8660" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8615" class="Function">em-irrefutable</a> <a id="8675" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8675" class="Bound">k</a> <a id="8677" class="Symbol">=</a> <a id="8679" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8675" class="Bound">k</a> <a id="8681" class="Symbol">(</a><a id="8682" href="https://agda.github.io/agda-stdlib/Data.Sum.Base.html#500" class="InductiveConstructor">inj₂</a> <a id="8687" class="Symbol">λ{</a> <a id="8690" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8690" class="Bound">x</a> <a id="8692" class="Symbol">→</a> <a id="8694" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8675" class="Bound">k</a> <a id="8696" class="Symbol">(</a><a id="8697" href="https://agda.github.io/agda-stdlib/Data.Sum.Base.html#475" class="InductiveConstructor">inj₁</a> <a id="8702" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#8690" class="Bound">x</a><a id="8703" class="Symbol">)</a> <a id="8705" class="Symbol">})</a>{% endraw %}</pre>
The best way to explain this code is to develop it interactively.

    em-irrefutable k = ?

Given evidence `k` that `¬ (A ⊎ ¬ A)`, that is, a function that give a
value of type `A ⊎ ¬ A` returns a value of the empty type, we must fill
in `?` with a term that returns a value of the empty type.  The only way
we can get a value of the empty type is by applying `k` itself, so let's
expand the hole accordingly.

    em-irrefutable k = k ?

We need to fill the new hole with a value of type `A ⊎ ¬ A`. We don't have
a value of type `A` to hand, so let's pick the second disjunct.

    em-irrefutable k = k (inj₂ λ{ x → ? })

The second disjunct accepts evidence of `¬ A`, that is, a function
that given a value of type `A` returns a value of the empty type.  We
bind `x` to the value of type `A`, and now we need to fill in the hole
with a value of the empty type.  Once again, the only way we can get a
value of the empty type is by applying `k` itself, so let's expand the
hole accordingly.

    em-irrefutable k = k (inj₂ λ{ x → k ? })

This time we do have a value of type `A` to hand, namely `x`, so we can
pick the first disjunct.

    em-irrefutable k = k (inj₂ λ{ x → k (inj₁ x) })

There are no holes left! This completes the proof.

The following story illustrates the behaviour of the term we have created.
(With apologies to Peter Selinger, who tells a similar story
about a king, a wizard, and the Philosopher's stone.)

Once upon a time, the devil approached a man and made an offer:
"Either (a) I will give you one billion dollars, or (b) I will grant
you any wish if you pay me one billion dollars.
Of course, I get to choose whether I offer (a) or (b)."

The man was wary.  Did he need to sign over his soul?
No, said the devil, all the man need do is accept the offer.

The man pondered.  If he was offered (b) it was unlikely that he would
ever be able to buy the wish, but what was the harm in having the
opportunity available?

"I accept," said the man at last.  "Do I get (a) or (b)?"

The devil paused.  "I choose (b)."

The man was disappointed but not surprised.  That was that, he thought.
But the offer gnawed at him.  Imagine what he could do with his wish!
Many years passed, and the man began to accumulate money.  To get the
money he sometimes did bad things, and dimly he realised that
this must be what the devil had in mind.
Eventually he had his billion dollars, and the devil appeared again.

"Here is a billion dollars," said the man, handing over a valise
containing the money.  "Grant me my wish!"

The devil took possession of the valise.  Then he said, "Oh, did I say
(b) before?  I'm so sorry.  I meant (a).  It is my great pleasure to
give you one billion dollars."

And the devil handed back to the man the same valise that the man had
just handed to him.

(Parts of the above are adopted from "Call-by-Value is Dual to Call-by-Name",
Philip Wadler, _International Conference on Functional Programming_, 2003.)


#### Exercise `Classical` (stretch)

Consider the following principles.

  * Excluded Middle: `A ⊎ ¬ A`, for all `A`
  * Double Negation Elimination: `¬ ¬ A → A`, for all `A`
  * Peirce's Law: `((A → B) → A) → A`, for all `A` and `B`.
  * Implication as disjunction: `(A → B) → ¬ A ⊎ B`, for all `A` and `B`.
  * De Morgan: `¬ (¬ A × ¬ B) → A ⊎ B`, for all `A` and `B`.

Show that each of these implies all the others.


#### Exercise `Stable` (stretch)

Say that a formula is _stable_ if double negation elimination holds for it.
<pre class="Agda">{% raw %}<a id="Stable"></a><a id="12219" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#12219" class="Function">Stable</a> <a id="12226" class="Symbol">:</a> <a id="12228" class="PrimitiveType">Set</a> <a id="12232" class="Symbol">→</a> <a id="12234" class="PrimitiveType">Set</a>
<a id="12238" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#12219" class="Function">Stable</a> <a id="12245" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#12245" class="Bound">A</a> <a id="12247" class="Symbol">=</a> <a id="12249" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="12251" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#898" class="Function Operator">¬</a> <a id="12253" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#12245" class="Bound">A</a> <a id="12255" class="Symbol">→</a> <a id="12257" href="{% endraw %}{{ site.baseurl }}{% link out/plfa/Negation.md %}{% raw %}#12245" class="Bound">A</a>{% endraw %}</pre>
Show that any negated formula is stable, and that the conjunction
of two stable formulas is stable.

## Standard Prelude

Definitions similar to those in this chapter can be found in the standard library.
<pre class="Agda">{% raw %}<a id="12488" class="Keyword">import</a> <a id="12495" href="https://agda.github.io/agda-stdlib/Relation.Nullary.html" class="Module">Relation.Nullary</a> <a id="12512" class="Keyword">using</a> <a id="12518" class="Symbol">(</a><a id="12519" href="https://agda.github.io/agda-stdlib/Relation.Nullary.html#464" class="Function Operator">¬_</a><a id="12521" class="Symbol">)</a>
<a id="12523" class="Keyword">import</a> <a id="12530" href="https://agda.github.io/agda-stdlib/Relation.Nullary.Negation.html" class="Module">Relation.Nullary.Negation</a> <a id="12556" class="Keyword">using</a> <a id="12562" class="Symbol">(</a><a id="12563" href="https://agda.github.io/agda-stdlib/Relation.Nullary.Negation.html#688" class="Function">contraposition</a><a id="12577" class="Symbol">)</a>{% endraw %}</pre>

## Unicode

This chapter uses the following unicode.

    ¬  U+00AC  NOT SIGN (\neg)
