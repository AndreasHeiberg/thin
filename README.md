# Thin - Flat file content management

Thin is a package for flat file content management. By default, it supports
files in Markdown and Markdown Extra format.

## Installation

Install Thin from [Packagist](https://packagist.org/packages/petersuhm/thin):

```
"petersuhm/thin": "dev-master"
```

## Usage

You can make your own implementations, depending on your type of content and
which parser you want to use (it's Markdown by default). By default, Thin comes
with two parsers, one for Markdown and one for Markdown Extra, and one model to
handle blog posts.

```php
$postLoader = new \Thin\PostLoader;

$postLoader->config(array(
    'document_path' = '../posts',
    'document_ext' = '.md'
));

// Returns a DocumentCollection
$posts = $postLoader->all();

// You can limit the number of documents
$posts->limit(5);

// You can also order documents by metadata
$posts->orderBy('date', 'desc');
$posts->orderBy('date', 'desc')->limit(3);

foreach ($posts as $post)
{
    $post->getMetadata('title');
    $post->getContent();
    $post->getHtmlContent();

    // Metadata can be accessed dynamically as well:
    $post->title;

    // Optionally give an instance of ParserInterface as a parameter
    $post->getHtmlContent(new \Thin\Parsers\MarkdownExtraParser);

}

// The PostLoader uses a slug as identifier
$post = $postLoader->find('some-slug');
```

The PostLoader expects files to follow the following convention:

```
{
    "some_metadata": "In JSON format",
    "...": "..."
}

The content of the document.
```

If your files have a different format, you can write your own loader.
