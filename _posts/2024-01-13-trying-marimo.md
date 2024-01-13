---
title: Trying out Marimo (interactive Notebooks)
layout: post
---

When I was working on a presentation about using python for data analysis, I stumbled upon [Marimo](https://marimo.io) but it seemed to be a fairly new project. There have been a number of developments, so I think it's time to check it out.

I just wanted to explore a bit of what it does and features it has (not really *use* it), so I made I made a very simple notebook.

To get started I wanted to use poetry:

```bash
# first make and go into new directory
poetry new .
poetry add marimo
# after installation is finished
poetry shell
```

Now it's ready to start using:

```bash
marimo tutorial intro
```

**Initial Thoughts**

- I like that this has an easy interface (i.e. I don't have to deal with a full UI before I get into a notebook like Jupyter. Just type `marimo edit <file>` and we're good.
- I like that there are interactive elements. 
- The hidden content shows up under the cells (bug)

### What does the python file look like?

I'll make a hello_world.py with a markdown block and code printing "hello world".

```python
import marimo

__generated_with = "0.1.76"
app = marimo.App()


@app.cell
def __():
    import marimo as mo
    return mo,


@app.cell
def __(mo):
    mo.md("Hello World")
    return


@app.cell
def __():
    print("Hello World")
    return


@app.cell
def __():
    return


if __name__ == "__main__":
    app.run()
```

That's very simple. The nice thing about this is that it works great for git. Even just being able to edit this in a sensible manner outside of the browser interface is possible, although it doesn't update live that way (but I have seen that this is in the roadmap).

I can now run this as an app as well with `marimo run <file>`.

If I edit the notebook, the `print()` statement works, but when running the app it doesn't show up. The only thing that shows up in the app is the rendered "Hello World" from the markdown block.

*Note: the `print()` statement printed to the console when I ran it as an app.*


### What does the HTML output look like?

Part of the UI has `Export to HTML` as a button, but also as a command in the command palette.

Here's the output from the one above:

```HTML

<!DOCTYPE html>
<html><head><meta charset="utf-8">
<link rel="icon" href="https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist/favicon.ico" crossorigin="anonymous">
<!-- Preload is necessary because we show these images when we disconnect from the server,
but at that point we cannot load these images from the server -->
<link rel="preload" href="https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist/assets/gradient-Mh0FAv0A.png" as="image" crossorigin="anonymous">
<link rel="preload" href="https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist/assets/noise-OtAaEwPD.png" as="image" crossorigin="anonymous">
<!-- Preload the fonts -->






<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#000000">
<meta name="description" content="a marimo app">
<link rel="apple-touch-icon" href="https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist/apple-touch-icon.png" crossorigin="anonymous">
<link rel="manifest" href="https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist/manifest.json" crossorigin="anonymous">
<script data-marimo="true">
function __resizeIframe(obj) {
// Resize the iframe to the height of the content
obj.style.height =
obj.contentWindow.document.documentElement.scrollHeight + "px";
// Resize the iframe when the content changes
const resizeObserver = new ResizeObserver((entries) => {
obj.style.height =
obj.contentWindow.document.documentElement.scrollHeight + "px";
});
resizeObserver.observe(obj.contentWindow.document.body);
}
</script>

    
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="">
  <link href="https://fonts.googleapis.com/css2?family=Fira+Mono:wght@400;500;700&amp;family=Lora&amp;family=PT+Sans:wght@400;700&amp;display=swap" rel="stylesheet"></head>

    <body>
      <div id="root"></div>
      <marimo-mode data-mode="read" hidden=""></marimo-mode>
      <marimo-filename hidden="">hello_world.py</marimo-filename>
<marimo-version data-version="0.1.76" hidden=""></marimo-version>
<marimo-user-config data-config="{&quot;completion&quot;: {&quot;activate_on_typing&quot;: true, &quot;copilot&quot;: false}, &quot;display&quot;: {&quot;theme&quot;: &quot;light&quot;, &quot;code_editor_font_size&quot;: 14}, &quot;formatting&quot;: {&quot;line_length&quot;: 79}, &quot;keymap&quot;: {&quot;preset&quot;: &quot;default&quot;}, &quot;runtime&quot;: {&quot;auto_instantiate&quot;: true}, &quot;save&quot;: {&quot;autosave&quot;: &quot;after_delay&quot;, &quot;autosave_delay&quot;: 1000, &quot;format_on_save&quot;: false}, &quot;server&quot;: {&quot;browser&quot;: &quot;default&quot;}}" hidden=""></marimo-user-config>
<marimo-app-config data-config="{&quot;width&quot;: &quot;normal&quot;, &quot;layout_file&quot;: null}" hidden=""></marimo-app-config>
      <script data-marimo="true">
        window.__MARIMO_STATIC__ = {};
        window.__MARIMO_STATIC__.version = "0.1.76";
        window.__MARIMO_STATIC__.notebookState = {"cellIds":["0","1","2","3"],"cellData":{"0":"JTdCJTIyaWQlMjIlM0ElMjIwJTIyJTJDJTIyY29uZmlnJTIyJTNBJTdCJTIyZGlzYWJsZWQlMjIlM0FmYWxzZSUyQyUyMmhpZGVfY29kZSUyMiUzQWZhbHNlJTdEJTJDJTIybmFtZSUyMiUzQSUyMl9fJTIyJTJDJTIyY29kZSUyMiUzQSUyMmltcG9ydCUyMG1hcmltbyUyMGFzJTIwbW8lMjIlMkMlMjJlZGl0ZWQlMjIlM0FmYWxzZSUyQyUyMmxhc3RDb2RlUnVuJTIyJTNBbnVsbCUyQyUyMnNlcmlhbGl6ZWRFZGl0b3JTdGF0ZSUyMiUzQW51bGwlN0Q=","1":"JTdCJTIyaWQlMjIlM0ElMjIxJTIyJTJDJTIyY29uZmlnJTIyJTNBJTdCJTIyZGlzYWJsZWQlMjIlM0FmYWxzZSUyQyUyMmhpZGVfY29kZSUyMiUzQWZhbHNlJTdEJTJDJTIybmFtZSUyMiUzQSUyMl9fJTIyJTJDJTIyY29kZSUyMiUzQSUyMm1vLm1kKCU1QyUyMkhlbGxvJTIwV29ybGQlNUMlMjIpJTIyJTJDJTIyZWRpdGVkJTIyJTNBZmFsc2UlMkMlMjJsYXN0Q29kZVJ1biUyMiUzQW51bGwlMkMlMjJzZXJpYWxpemVkRWRpdG9yU3RhdGUlMjIlM0FudWxsJTdE","2":"JTdCJTIyaWQlMjIlM0ElMjIyJTIyJTJDJTIyY29uZmlnJTIyJTNBJTdCJTIyZGlzYWJsZWQlMjIlM0FmYWxzZSUyQyUyMmhpZGVfY29kZSUyMiUzQWZhbHNlJTdEJTJDJTIybmFtZSUyMiUzQSUyMl9fJTIyJTJDJTIyY29kZSUyMiUzQSUyMnByaW50KCU1QyUyMkhlbGxvJTIwV29ybGQlNUMlMjIpJTIyJTJDJTIyZWRpdGVkJTIyJTNBZmFsc2UlMkMlMjJsYXN0Q29kZVJ1biUyMiUzQW51bGwlMkMlMjJzZXJpYWxpemVkRWRpdG9yU3RhdGUlMjIlM0FudWxsJTdE","3":"JTdCJTIyaWQlMjIlM0ElMjIzJTIyJTJDJTIyY29uZmlnJTIyJTNBJTdCJTIyZGlzYWJsZWQlMjIlM0FmYWxzZSUyQyUyMmhpZGVfY29kZSUyMiUzQWZhbHNlJTdEJTJDJTIybmFtZSUyMiUzQSUyMl9fJTIyJTJDJTIyY29kZSUyMiUzQSUyMiUyMiUyQyUyMmVkaXRlZCUyMiUzQWZhbHNlJTJDJTIybGFzdENvZGVSdW4lMjIlM0FudWxsJTJDJTIyc2VyaWFsaXplZEVkaXRvclN0YXRlJTIyJTNBbnVsbCU3RA=="},"cellRuntime":{"0":"JTdCJTIyb3V0bGluZSUyMiUzQW51bGwlMkMlMjJvdXRwdXQlMjIlM0ElN0IlMjJjaGFubmVsJTIyJTNBJTIyb3V0cHV0JTIyJTJDJTIybWltZXR5cGUlMjIlM0ElMjJ0ZXh0JTJGcGxhaW4lMjIlMkMlMjJkYXRhJTIyJTNBJTIyJTIyJTJDJTIydGltZXN0YW1wJTIyJTNBMTcwNTEyODg1OC4zODEyMzQlN0QlMkMlMjJjb25zb2xlT3V0cHV0cyUyMiUzQSU1QiU1RCUyQyUyMnN0YXR1cyUyMiUzQSUyMmlkbGUlMjIlMkMlMjJpbnRlcnJ1cHRlZCUyMiUzQWZhbHNlJTJDJTIyZXJyb3JlZCUyMiUzQWZhbHNlJTJDJTIyc3RvcHBlZCUyMiUzQWZhbHNlJTJDJTIycnVuRWxhcHNlZFRpbWVNcyUyMiUzQTAuMzY0MDY1MTcwMjg4MDg1OTQlMkMlMjJydW5TdGFydFRpbWVzdGFtcCUyMiUzQW51bGwlN0Q=","1":"JTdCJTIyb3V0bGluZSUyMiUzQSU3QiUyMml0ZW1zJTIyJTNBJTVCJTVEJTdEJTJDJTIyb3V0cHV0JTIyJTNBJTdCJTIyY2hhbm5lbCUyMiUzQSUyMm91dHB1dCUyMiUyQyUyMm1pbWV0eXBlJTIyJTNBJTIydGV4dCUyRmh0bWwlMjIlMkMlMjJkYXRhJTIyJTNBJTIyJTNDc3BhbiUyMGNsYXNzJTNEJ21hcmtkb3duJyUzRSUzQ3NwYW4lMjBjbGFzcyUzRCdwYXJhZ3JhcGgnJTNFSGVsbG8lMjBXb3JsZCUzQyUyRnNwYW4lM0UlM0MlMkZzcGFuJTNFJTIyJTJDJTIydGltZXN0YW1wJTIyJTNBMTcwNTEyODg1OC40MDY0NjIlN0QlMkMlMjJjb25zb2xlT3V0cHV0cyUyMiUzQSU1QiU1RCUyQyUyMnN0YXR1cyUyMiUzQSUyMmlkbGUlMjIlMkMlMjJpbnRlcnJ1cHRlZCUyMiUzQWZhbHNlJTJDJTIyZXJyb3JlZCUyMiUzQWZhbHNlJTJDJTIyc3RvcHBlZCUyMiUzQWZhbHNlJTJDJTIycnVuRWxhcHNlZFRpbWVNcyUyMiUzQTI0Ljg2OTkxODgyMzI0MjE4OCUyQyUyMnJ1blN0YXJ0VGltZXN0YW1wJTIyJTNBbnVsbCU3RA==","2":"JTdCJTIyb3V0bGluZSUyMiUzQW51bGwlMkMlMjJvdXRwdXQlMjIlM0ElN0IlMjJjaGFubmVsJTIyJTNBJTIyb3V0cHV0JTIyJTJDJTIybWltZXR5cGUlMjIlM0ElMjJ0ZXh0JTJGcGxhaW4lMjIlMkMlMjJkYXRhJTIyJTNBJTIyJTIyJTJDJTIydGltZXN0YW1wJTIyJTNBMTcwNTEyODg1OC4zODA1NSU3RCUyQyUyMmNvbnNvbGVPdXRwdXRzJTIyJTNBJTVCJTdCJTIyY2hhbm5lbCUyMiUzQSUyMnN0ZG91dCUyMiUyQyUyMm1pbWV0eXBlJTIyJTNBJTIydGV4dCUyRnBsYWluJTIyJTJDJTIyZGF0YSUyMiUzQSUyMkhlbGxvJTIwV29ybGQlNUNuJTIyJTJDJTIydGltZXN0YW1wJTIyJTNBMTcwNTEyODg1OC4zOTI2OTYxJTdEJTVEJTJDJTIyc3RhdHVzJTIyJTNBJTIyaWRsZSUyMiUyQyUyMmludGVycnVwdGVkJTIyJTNBZmFsc2UlMkMlMjJlcnJvcmVkJTIyJTNBZmFsc2UlMkMlMjJzdG9wcGVkJTIyJTNBZmFsc2UlMkMlMjJydW5FbGFwc2VkVGltZU1zJTIyJTNBMC4zODIxODQ5ODIyOTk4MDQ3JTJDJTIycnVuU3RhcnRUaW1lc3RhbXAlMjIlM0FudWxsJTdE","3":"JTdCJTIyb3V0bGluZSUyMiUzQW51bGwlMkMlMjJvdXRwdXQlMjIlM0ElN0IlMjJjaGFubmVsJTIyJTNBJTIyb3V0cHV0JTIyJTJDJTIybWltZXR5cGUlMjIlM0ElMjJ0ZXh0JTJGcGxhaW4lMjIlMkMlMjJkYXRhJTIyJTNBJTIyJTIyJTJDJTIydGltZXN0YW1wJTIyJTNBMTcwNTEyODg1OC4zNzk4NDMlN0QlMkMlMjJjb25zb2xlT3V0cHV0cyUyMiUzQSU1QiU1RCUyQyUyMnN0YXR1cyUyMiUzQSUyMmlkbGUlMjIlMkMlMjJpbnRlcnJ1cHRlZCUyMiUzQWZhbHNlJTJDJTIyZXJyb3JlZCUyMiUzQWZhbHNlJTJDJTIyc3RvcHBlZCUyMiUzQWZhbHNlJTJDJTIycnVuRWxhcHNlZFRpbWVNcyUyMiUzQTAuMzM3MTIzODcwODQ5NjA5NCUyQyUyMnJ1blN0YXJ0VGltZXN0YW1wJTIyJTNBbnVsbCU3RA=="}};
        window.__MARIMO_STATIC__.assetUrl = "https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist";
        window.__MARIMO_STATIC__.files = {};
      </script>

      <marimo-code hidden="">
        import%20marimo%0A%0A__generated_with%20%3D%20%220.1.76%22%0Aapp%20%3D%20marimo.App()%0A%0A%0A%40app.cell%0Adef%20__()%3A%0A%20%20%20%20import%20marimo%20as%20mo%0A%20%20%20%20return%20mo%2C%0A%0A%0A%40app.cell%0Adef%20__(mo)%3A%0A%20%20%20%20mo.md(%22Hello%20World%22)%0A%20%20%20%20return%0A%0A%0A%40app.cell%0Adef%20__()%3A%0A%20%20%20%20print(%22Hello%20World%22)%0A%20%20%20%20return%0A%0A%0A%40app.cell%0Adef%20__()%3A%0A%20%20%20%20return%0A%0A%0Aif%20__name__%20%3D%3D%20%22__main__%22%3A%0A%20%20%20%20app.run()
      </marimo-code>
    
  
  <script type="module" crossorigin="anonymous" src="https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist/assets/index-nZBIgTj0.js"></script><link rel="stylesheet" crossorigin="anonymous" href="https://cdn.jsdelivr.net/npm/@marimo-team/frontend@0.1.76/dist/assets/index-Qg8Acq0C.css"></body></html>
```


That's a lot of stuff. It's also not interactive until they get some WASM magic going. One thing that I was curious about is how I can do something in Marimo and then incorporate it into a page (like on this site). Jupyter allows me to export as markdown, and then I can go from there.

Perhaps Marimo is trying to do a lot more and keep the interface consistent. That's nice, but it would be great if I can export as markdown.

### Sharing a notebook

If you click on the share notebook button, it shows this:

```text
Share static notebook
You can publish a static, non-interactive version of this notebook to the public web. We will create a link for you that lives on https://static.marimo.app.
```

That's interesting, but that instantly think about costs involved. Any time I see things like this, I start to wonder where the money comes from and that this free tool will not be free for much longer.


### Concluding thoughts

I like this a lot. It's a little bit buggy, but the way the demos look and the interactive parts of this mean it's going to be really great.


