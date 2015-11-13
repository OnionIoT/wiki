[[_TOC_]]

### Navigate this page:
1. [Setting page titles](#subpages)
1. [Using subpages](#subpages)
1. [Page sanitization (security note)](#page-sanitization)
1. [Gollum tag reference](#tags)
1. [Gollum code block reference](#code-blocks)
1. [Gollum macro reference](#macros)
1. [Gollum diagram reference](#diagrams)
1. [Integrating mathematics](#mathematics)

# PAGE TITLES

By default, a page's title will be its file path, relative to repository root (or page file dir), without extension (e.g. `/mordor/Sauron`).

### Custom titles via CLI

If you start Gollum with the `--h1-title` option, then the first `<h1>` header on the page will be used. So, for example, this markdown page:

```markdown
# This is my title
# This is the first h1 that will be displayed
```

will be rendered with the `This is my title` page title. The first header rendered in the page's text will be `This is the first h1 that will be displayed`.

This option overrides the metadata syntax.

### Custom titles via the metadata syntax

The syntax:
```
<!-- --- title: My page title -->
```

Note that the metadata must be the first piece of text on the page.

# SUBPAGES

Gollum allows you to add a header, sidebar and footer to pages. They will most probably be shared among several pages. Subpages affect all pages in their directory and any subdirectories that do not have a subpage of their own.

1. Header files are named `_Header.ext`, where `ext` is one of the supported formats' extensions.
2. Sidebar files are named `_Sidebar.ext`, where `ext` is one of the supported formats' extensions.
3. Footer files are named `_Footer.ext`, where `ext` is one of the supported formats' extensions.

# PAGE SANITIZATION

For security and compatibility reasons, Gollum pages may not contain custom CSS, JavaScript, and potentially dangerous or unknown HTML markup. They will be stripped from the generated HTML. See the [Security](https://github.com/gollum/gollum/wiki/Security#page-sanitization) page for more information.

# TAGS

Gollum's tags have been made to resemble the tags of other markups, especially MediaWiki. The basic syntax is:
```
[[tag]]
```

Some tags will accept attributes which are separated by pipe symbols. Some attributes must precede the tag and some must follow it:
* `[[prefix-attribute|tag]]`
* `[[tag|suffix-attribute]]`

## Link tag

The link tag creates a clickable link to a resource. It has two forms:
* `[[linked-resource]]` - the created link's text will be equal to the resource's text
* `[[link-text|linked-resource]]` - the created link's text will be `link-text`

### Linking external resources

Examples:
* `[[http://example.com]]`
* `[[link-text|http://example.com/pdfs/gollum.pdf]]`

Allowed URL protocols are listed [here](https://github.com/gollum/gollum/wiki/Security#allowed-url-protocols).

### Linking internal files (not images or pages)

Identical to linking external resources, but the URLs must be paths:
* Either a relative path, e.g. `docs/diagram.png`. Relative paths point to a file relative to the page, where the link is defined. They must **not** be prefixed with a slash.
* Or an absolute path, e.g. `/docs/diagram.png`. Absolute paths point to a file relative to Gollum repository's root. They must be prefixed with a slash.

### Linking internal images

Identical to linking internal files, but also supports several special attributes:

1. `[[image-url|alt=text]]`
    * Text to display when the image is not found.
2. `[[image-url|frame]]`
    * Tells Gollum to place the image in an `<iframe>`.
3. `[[image-url|align=position]]`
    * Tells Gollum to align the image in the given way. Position can be `left`, `center`, and `right`. Default: `left`.
4. `[[image-url|float]]`
    * Tells Gollum to float the image so that the text flows around it. This option can not be used with the `align=center` option. When floating is enabled and alignment is not specified, `align=left` is applied.
5. `[[image-url|height=value]]`
    * Set maximum height for the image. Values must be specified either with `px` or `em` unit.
6. `[[image-url|width=value]]`
    * Set maximum width for the image. Values must be specified either with `px` or `em` unit.

All of these attributes can be combined together simply by separating them with pipes.

**Special behaviour:**

1. `[[image-url|frame|alt=text]]`
    * Tells Gollum to place the image in an `<iframe>` and uses the `alt`'s text as its caption.

**TODO:**  
By default text will fill up all the space around the image. To control how
much should show up use this tag to stop and start a new block so that
additional content doesn't fill in.

```
[[_]]
```

### Linking internal pages

The following tag will create a link to another Gollum page:

```
[[cannonical-page-filename]]
```

Cannonical page filename is the page's filename, but after applying these rules:
* Extension is tossed away.
* Spaces (U+0020) are replaced with dashes (U+002D).
* Forward slashes (U+002F) are replaced with dashes (U+002D).

**Examples:**
* `[[J. R. R. Tolkien]]` will reference the `J.-R.-R.-Tolkien.ext` page
* `[[Movies / The Hobbit]]` will reference the `Movies---The-Hobbit.ext` page
* `[[モルドール]]` will reference the `モルドール.ext` page

The referenced page may exist anywhere in the git repository. Gollum will search for it and link the first such page that it finds.

## Include tag

As opposed to the link tag, this one is dedicated to pages, has a special syntax and includes a page into another, rather than linking it:
```
[[include:cannonical-page-filename]]
```

## Table-of-contents (TOC) tag

Gollum provides a special tag to embed table of contents into a page:
```
[[_TOC_]]
```

**Notes:**
* It is case sensitive, so always use uppercase.
* It can also be inserted into subpages.

There is also a global TOC setting (disabled by default) that forces TOC into every page. Simply add the following line to your [configuration file](https://github.com/gollum/gollum#config-file):  
```ruby
Precious::App.set(:wiki_options, { :universal_toc => true })
```

## Escaping tags

In case a tag needs to be displayed on a page literally, preface the tag with a single quote:
```
''[[tag]]
```

# CODE BLOCKS

## Fenced code blocks (GitHub style)

Naturally, all pages can benefit from this syntax. Example:
    
    ```
    def foo
        puts 'bar'
    end
    ```

The block must be enclosed with three backticks.

**Indentation:**
* The opening backticks can be indented arbitrarily.
* The block's content should be indented at the same level as the opening backticks. If it is indented with an additional two spaces or one tab, they will be ignored (this makes the blocks easier to read in plaintext).
* The ending backticks must be indented at the same level as the opening backticks.

## Fenced code blocks (Kramdown style)

Naturally, all pages can benefit from this syntax. Example:

    ~~~~~~~~
    Here comes some code.
    ~~~~~~~~

The block must be enclosed with three or more tildes (~).

## Syntax highlighted code blocks

This is an extension to GitHub-style code blocks. Naturally, all pages can benefit from this syntax. Example:

    ```ruby
    def foo
        puts 'bar'
    end
    ```

**TODO:** verify and update this + maybe it's time to recap (https://rubygems.org/search?utf8=%E2%9C%93&query=pygments)

As you can see, the code's language/markup is defined next to the opening backticks. There are a variety of languages/markups to use and they depend on which syntax highlighter is currently used in Gollum:
* [Rouge](https://github.com/jayferd/rouge)
    * This is the default, unless Python and [Pygments](https://rubygems.org/gems/pygments.rb) is installed in your system. Then, Pygments have more priority.
    * [Language list for Rouge](https://github.com/jneen/rouge/wiki/List-of-supported-languages-and-lexers).
* [Pygments](https://rubygems.org/gems/pygments.rb)
    * Requires Python 2.5+ (2.7.x is recommended).
    * [Language list for Pygments](http://pygments.org/docs/lexers/).

Additionally, you can syntax highlight a file from your repository. Naturally, when the file is updated and the page refreshed in browser, the code snippet is updated as well:
    
    ```ruby:/lib/gollum/app.rb```

In this case, the `/lib/gollum/app.rb` file will be included in the page, and syntax highlighted.

# MACROS

Beside tags, Gollum provides another layer of its own markup - macros. The main differences are:
* Tags are all predefined and can not be customized. Macros can be customized and you can even create your own.
* Different syntax.

## Default Macros

**AllPages**  
* Description: Prints a list of all wiki pages.
* Syntax: `<<AllPages()>>`
* Result (example): `<ul id="pages"><li>AllPagesMacroPage</li></ul>`

## Custom Macros

Each Macro must:

1. Subclass the [abstract Macro class](https://github.com/gollum/gollum-lib).
2. Override the `render` method.
3. Be registered in Gollum. Therefore:
    * either start Gollum with the `--config` option and put your custom Macro class in the `config.rb` file,
    * or start Gollum via Rack and put your custom Macro class in the `config.ru` file.

For example, look at the implementation of the [AllPages Macro](https://github.com/gollum/gollum-lib/blob/master/lib/gollum-lib/macro/all_pages.rb).

**Notes:**
* If you have created a useful Macro, please consider making a pull request to [gollum-lib](https://github.com/gollum/gollum-lib).
* If you have an idea for a useful Macro, please consider creating an issue for [gollum](https://github.com/gollum/gollum).

# DIAGRAMS

## Sequence diagrams

Gollum can render sequence diagrams from source on a page. Courtesy of [WebSequenceDiagrams](http://www.websequencediagrams.com). For example:
```
{{{{{{ blue-modern
    alice->bob: Test
    bob->alice: Test response
}}}}}}
```

You can replace the `blue-modern` style with any other supported style. Open the link above to discover what styles are available.

## PlantUML diagrams

If you install and configure your own [PlantUML server](https://github.com/gollum/gollum/wiki/Custom-PlantUML-Server), Gollum can render PlantUML diagrams from source on a page. Courtesy of [PlantUML](http://plantuml.sourceforge.net/). For example:
```
@startuml
Alice -> Bob: Authentication Request
Bob --> Alice: Authentication Response

Alice -> Bob: Another authentication Request
Alice <-- Bob: another authentication Response
@enduml
```

You can embed any of the diagram types supported by PlantUML: sequence diagrams, case diagrams, class diagrams, activity diagrams, component diagrams, object diagrams and even wireframe diagrams.

**Notes:**
* For wireframe diagrams, you must use the `*@startuml*` tag followed by the `*salt*` keyword syntax. The new `*@salt*` syntax is not supported. See [link](http://plantuml.sourceforge.net/salt.html).

# MATHEMATICS

To enable mathematical expressions in Gollum, start it with the `--mathjax` option. We recommend [this page](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference) to learn about the syntax.

**Examples:**
* Inline math:  
    ```
    \\\(2^2\\\)
    ```
* Display math:  
    ```text
    $$2^2$$
    \\\[2^2\\\]
    ```

By default, Gollum uses the `TeX-AMS-MML_HTMLorMML` config with the `autoload-all` extension. You can also set your own mathjax configuration with the `--mathjax-config` option.

**Notes:**
* When using the `Org-mode` markup, you will have to wrap display math blocks in `#+BEGIN_HTML ... #+END_HTML` blocks.