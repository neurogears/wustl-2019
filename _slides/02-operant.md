---
layout: slides
title: 2. Operant Behavior
permalink: /slides/operant/
---

<section data-markdown data-separator="^\n---\n$" data-separator-vertical="^\n--\n$">
<script type="text/template">

![Bonsai](../../assets/images/bonsai-lettering.svg)

### Operant Behavior
[bonsai-rx.org](http://bonsai-rx.org)

---

<!-- .element: data-transition="default none" -->
###### Transform

![Transform](../../assets/images/transform.svg)

--

<!-- .element: data-transition="default none" -->
###### Select

![Select](../../assets/images/select.svg)

--

<!-- .element: data-transition="none default" -->
###### SelectMany

![SelectMany](../../assets/images/selectmany.svg)

--

<!-- .element: data-transition="none default" -->
###### SelectMany: Play audio on cue

![SelectMany](../../assets/images/selectmany-playsound-1.svg)

--

<!-- .element: data-transition="none default" -->
###### SelectMany: Play audio on cue

![SelectMany](../../assets/images/selectmany-playsound-2.svg)

---

<!-- .element: data-transition="default none" -->
###### Buffer

![Buffer](../../assets/images/buffer.svg)

--

<!-- .element: data-transition="none default" -->
###### Buffer: Moving Average

![SelectMany](../../assets/images/buffer-movingaverage.svg)

---

<!-- .element: data-transition="default none" -->
###### TriggeredBuffer

![TriggeredBuffer](../../assets/images/triggeredbuffer.svg)

--

<!-- .element: data-transition="none default" -->
###### TriggeredBuffer: Signal Snapshot

![SelectMany](../../assets/images/triggeredbuffer-snapshot.svg)

---

###### Window

![Window](../../assets/images/window.svg)

---

<!-- .element: data-transition="default none" -->
###### TriggeredWindow

![TriggeredWindow](../../assets/images/triggeredwindow.svg)

--

<!-- .element: data-transition="none default" -->
###### TriggeredWindow: Record triggered video

![SelectMany](../../assets/images/triggeredwindow-recordclip.svg)

---

### Higher-Order Operators

![Concatenate video files using first order operators](../../assets/images/concatfile-firstorder.svg)

--

###### Enumerate Files

![Enumerate all file names in a folder](../../assets/images/concatfile-enumeratefiles.svg)

--

###### Create Observable

![Create sequences of frames from file names](../../assets/images/concatfile-observable.svg)

--

###### Concat

![Combine all sequences of frames into a single sequence](../../assets/images/concatfile-combine.svg)

--

###### Higher-Order: Batch concatenate multiple videos

![SelectMany](../../assets/images/higherorder-concatfiles.svg)

---

### Representing discrete states

**State**
<!-- .element: class="fragment" data-fragment-index="1" style="display: inline-block; vertical-align: middle;" -->

<small>"the particular condition that someone or something is in at a specific time"</small>
<!-- .element: class="fragment" data-fragment-index="1" style="display: inline-block; vertical-align: middle;" -->
<small>"a physical condition as regards internal or molecular form or structure"</small>
<!-- .element: class="fragment" data-fragment-index="2" style="display: inline-block; vertical-align: middle;" -->

**Event**
<!-- .element: class="fragment" data-fragment-index="3" style="display: inline-block; vertical-align: middle;" -->

<small>"a thing that happens or takes place, especially one of importance"</small>
<!-- .element: class="fragment" data-fragment-index="3" style="display: inline-block; vertical-align: middle;" -->
<small>"a single occurrence of a process, e.g. the ionization of one atom"</small>
<!-- .element: class="fragment" data-fragment-index="4" style="display: inline-block; vertical-align: middle;" -->

<small>source: <a href="https://en.oxforddictionaries.com/">Oxford English Living Dictionaries</a></small>
<!-- .element: class="fragment" data-fragment-index="1" style="display: inline-block; position: absolute; right: 0px;" -->

--

#### Working Definition

**State** → Extended

**Event** → Punctate

---

<!-- .element: data-transition="default none" -->
![SelectMany](../../assets/images/selectmany-events-hidden.svg)

--

<!-- .element: data-transition="none none" -->
![SelectMany](../../assets/images/selectmany-events-in.svg)

--

<!-- .element: data-transition="none none" -->
![SelectMany](../../assets/images/selectmany-states.svg)

--

<!-- .element: data-transition="none default" -->
![SelectMany](../../assets/images/selectmany-events-out.svg)

---

###### Merge

![Merge](../../assets/images/merge.svg)

---

###### Amb

![Amb](../../assets/images/amb.svg)

---

###### Concat

![Concat](../../assets/images/concat.svg)

---

### Sharing observable sequences

![Branching](../../assets/images/branching-simple.svg)
<!-- .element: style="display: inline-block; vertical-align: top;" -->
![Subjects (Publish)](../../assets/images/subjects-publish-simple.svg)
<!-- .element: class="fragment" style="display: inline-block; vertical-align: top; padding-left: 120px;" -->

--

### Sharing observable sequences

![Publish](../../assets/images/publish.svg)
<!-- .element: style="display: inline-block; vertical-align: top;" -->
![Replay](../../assets/images/replay.svg)
<!-- .element: class="fragment" style="display: inline-block; vertical-align: top; padding-left: 40px;" -->

--

### Sharing observable sequences

![Subjects (Publish)](../../assets/images/subjects-publish.svg)
<!-- .element: style="display: inline-block; vertical-align: top; padding-left: 120px;" -->
![Subjects (Replay)](../../assets/images/subjects-replay.svg)
<!-- .element: class="fragment" style="display: inline-block; vertical-align: top; padding-left: 120px;" -->

</script>
</section>