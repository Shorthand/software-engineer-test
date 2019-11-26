# Semantic structure from heading elements and semantic-structural incongruence in web pages

## A take-home challenge for software engineers.

### Background

In web pages, heading elements (`h1-h6`) are used to impose semantic structure on the content appearing in the page. They can be used to break an article into chapters or sections, with `h1` being a top-level heading, `h2` being the heading one level down and so on. In other words the semantics of the heading elements arise from the weight they carry in relation to one another. However, there is no explicit containment hierarchy between these headings. Thus, it is the responsibility of the page author or generator to use heading elements in a semantically appropriate way.

Often, heading elements appear within the context of structural elements, such as `<div>` elements that have no real semantics attached to them, or structural elements, such as `<section>`, which *do* carry meaning within the structure of a web page. Sometimes, an incongruence or conflict emerges between the innate semantics of the heading elements and imposed semantics due to the use of nested structural elements.

For instance, consider this simple scenario:

```
<section>
  <h2>Heading 2</h2>
  <h3>Heading 3</h3>
  <section>
    <h2>Another Heading 2</h2>
  </section>
</section>
```

This is perfectly valid HTML, but semantically, it's confused because we have a `h2` element nested at a deeper level than a `h3` element that precedes it.

### Challenge

Your challenge, should you choose to accept it, is two-fold:
1. Extract the semantic structure of any web page implied by heading elements `h1-h6`. The result should be a, possibly multi-rooted, tree structure. For example, the sequence `h1`, `h2`, `h3`, `h4`, `h2`, `h5` yields the tree `[h1, [h2, h2, [h3, [h4]], h2, [h5]]]` assuming a pre-ordered notation. Represent each heading as a key-value pair denoting the header tag and its content, like `"h1": "Heading 1"`. For this part of the task you can ignore any other elements in the page.
1. Check the extracted semantic structure against the actual containment structure of the page, recording any incongruence by adding the out-of-place heading element to an array. For example, if the above heading sequence is shown in the context of the following structure, `<section>, <h1>, <h2>, <h2>, <h3>, <h4>, <section>, <h2>, <h5>, </section>, </section>`, the final `h2` element would be added to the array because the container structure puts that `h2` element in a nested position relative to the position of the `h4` element that precedes it, despite the `h2` element carrying more semantic weight. Use the same key-value representation as above.

Structure your code so that it can:
1. be run as a standalone command on the command line, where the URL to process is given as an argument, e.g., `checkheadings https://foo.com`
1. be run as a little web app that takes a URL as a URL parameter, e.g., `http://localhost:8000?u=https://foo.com`

In both cases, the result should be a well-formed JSON object encapsulating the outputs of the two parts of the task above. For example, given the above example, the response might look something like this:
```
{
  "semantic-structure": {
    [
      {"h1": "Heading 1"},
      [
        {"h2": "Heading 2"},
        {"h2": "Another Heading 2"},
        [
          {"h3": "Heading 3"},
          [
            {"h4": "Heading 4"}
          ]
        ],
        {"h2": "An out of place Heading 2"},
        [
          {"h5": "Heading 5"}
        ]
      ]
    ]
  },
  "incongruent-headings": {
    [{"h2": "An out of place Heading 2"}]
  }
}
```

Error conditions, such as a malformed URL given as argument, should be reported appropriately for both command-line and web API invocations.

Add a description of your approach to the bottom of this README, including a note about the computation complexity of your solution. Your description should also include instructions for running your solution in web app and command line form.

### Notes

1. You may assume web pages are static. That is, you do not need to evaluate inline or referenced javascript before processing the page, though you might like to briefly comment on how you'd modify your solution to support single-page websites and other dynamically generated pages.
1. Tests are not required, but your code should be written, structured and commented as if it would be deployed to production.
1. You may use any language, libraries, frameworks that you wish, but please ensure you've documented clearly how to install anything that's required to run your solution.
1. We would not expect you to spend more than two hours completing this challenge.
1. If you need any of the requirements to be clarified, please just ask!

### Criteria

Assuming that you submit a "correct" solution, we are then first and foremost interested to see *how* you solve the problem. That is our top criterion below.

1. Elegance/simplicity of your solution
1. Elegance/readability of your code (note: this is very distinct from the first criterion!)
1. Documentation and comments

### Submitting your solution

Fork this repository, create your solution in your fork, then open a pull request against this (origin) repo and let us know that you've completed the challenge.
