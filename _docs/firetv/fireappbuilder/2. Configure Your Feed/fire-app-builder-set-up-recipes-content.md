---
title: Set Up the Content Recipe
permalink: fire-app-builder-set-up-recipes-content.html
sidebar: fireappbuilder
product: Fire App Builder
toc-style: kramdown
github: true
---

 When you [configured your categories][fire-app-builder-set-up-recipes-categories], you configured the general groupings for your media. In this step you will map your feed's content (the titles, descriptions, images, video URLs, and so on) to the Fire App Builder content model. For an overview to recipe configuration, see [Recipe Configuration Overview][fire-app-builder-set-up-recipes-overview].

* TOC
{:toc}

## Configure the Content Recipe

1.  Open the **LightCastContentsRecipe.json** file (located in **app > assets > recipes**).
2.  Configure the file's values as explained in the following table. If parameters need more explanation, the sections below the table provide more detail.

    <table class="grid">
    <colgroup>
    <col width="20%" />
    <col width="80%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Key</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td markdown="1">`cooker`
    </td>
    <td markdown="1">{% include_relative recipe_cooker.md %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`format`
    </td>
    <td markdown="1">{% include_relative recipe_format.md %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`translator`
    </td>
    <td markdown="1">{% include_relative recipe_translator.md default_value="`ContentTranslator`" %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`model`
    </td>
    <td markdown="1">Specifies the content model for the data. The content model provides the structure for your content and maps it into the Fire App Builder UI. Leave it at the default: `com.amazon.android.model.content.Content`.
    </td>
    </tr>

    <tr>
    <td markdown="1">`modelType`
    </td>
    <td markdown="1">{% include_relative recipe_modeltype.md %}
    </td>
    </tr>

    <tr>
    <td markdown="1">`query`
    </td>
    <td markdown="1">{% include_relative recipe_query.md type="content" %}
    </td>
    </tr>

    <tr>
    <td markdown="1">`queryResultType`
    </td>
    <td markdown="1">{% include_relative recipe_queryresulttype.md %}
    </td>
    </tr>

    <tr>
    <td markdown="1">`matchList`
    </td>
    <td markdown="1">{% include_relative recipe_matchlist.md %}
    </td>
    </tr>
    </tbody>
    </table>

### query Parameter {#queryparameter}

The syntax for JSON feeds differs significantly from the syntax for XML feeds, so the two formats are treated separately in the following sections:

* [JSON Feeds](#queryjson)
* [XML Feeds](#queryxml)

{% include tip.html content="Although the `query` syntax in this section might seem a little complex, remember that Fire App Builder lets you use any feed structure you want, without limiting you to a specific order or specification. With this flexibility, it's unavoidable that you'll need to use more advanced query syntax to target the elements in your feed." %}

#### JSON Feeds {#queryjson}

In the sample app in Fire App Builder, the value for `query` is `$[?(@.categories[0] in [$$par0$$])]`. As with the `query` parameter in the Categories recipe, this syntax is (mostly) [Jayway JsonPath](https://github.com/jayway/JsonPath) syntax. This syntax uses a [Jayway JsonPath filter operator](https://github.com/jayway/JsonPath#filter-operators) to select the items inside the `categories` array that have at least one item at the 0 position.

Let's take a step back to unpack this syntax with more clarity, because you'll need to customize this query to fit your own feed syntax. A sample [Lightcast feed](http://www.lightcast.com/api/firetv/channels.php?app_id=263&app_key=4rghy65dcsqa&action=channels_videos) (which is what the sample Fire App Builder uses) looks like this:

```json
[
  {
    "id": "136211",
    "title": "Legends Beach Resort, Negril Jamaica",
    "description": "Legends Beach Resort, Negril Jamaica",
    "duration": "538",
    "thumbURL": "http:\/\/l3.cdn01.net\/_thumbs\/0000136\/0136211\/0136211__009f.jpg",
    "imgURL": "http:\/\/l3.cdn01.net\/_thumbs\/0000136\/0136211\/0136211__009f.jpg",
    "videoURL": "http:\/\/media.cdn01.net\/802E1F\/process\/encoded\/video_1880k\/0000136\/0136211\/M3FA8J1MI.mp4?source=firetv&channel_id=13671",
    "categories": [
      "Jamaican Attractions"
    ],
    "channel_id": "13671"
  },
  {
    "id": "136216",
    "title": "Falmouth Jamaica Nature's Lullaby ",
    "description": "Falmouth Jamaica Nature's Lullaby",
    "duration": "1813",
    "thumbURL": "http:\/\/l4.cdn01.net\/_thumbs\/0000136\/0136216\/0136216__001f.jpg",
    "imgURL": "http:\/\/l4.cdn01.net\/_thumbs\/0000136\/0136216\/0136216__001f.jpg",
    "videoURL": "http:\/\/media.cdn01.net\/802E1F\/process\/encoded\/video_1880k\/0000136\/0136216\/L2XGJI1LM.mp4?source=firetv&channel_id=13672",
    "categories": [
      "The Country Jamaica"
    ],
    "channel_id": "13672"
  },
  {
    "id": "136209",
    "title": "Rafting on the Rio Grande River, Jamaica",
    "description": "Rafting on the Rio Grande River, Jamaica",
    "duration": "660",
    "thumbURL": "http:\/\/l1.cdn01.net\/_thumbs\/0000136\/0136209\/0136209__010f.jpg",
    "imgURL": "http:\/\/l1.cdn01.net\/_thumbs\/0000136\/0136209\/0136209__010f.jpg",
    "videoURL": "http:\/\/media.cdn01.net\/802E1F\/process\/encoded\/video_1880k\/0000136\/0136209\/I9BDFF0IM.mp4?source=firetv&channel_id=13671",
    "categories": [
      "Jamaican Attractions"
    ],
    "channel_id": "13671"
  }
]
```

(This isn't the full feed, but it shows the repeating structure.)

Plug this feed into the [Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/). Then run the following query:

```
$[?(@.categories[0] in ["The Country Jamaica"])]
```

This query returns the following:

```json
[
   {
      "id" : "136216",
      "title" : "Falmouth Jamaica Nature's Lullaby ",
      "description" : "Falmouth Jamaica Nature's Lullaby",
      "duration" : "1813",
      "thumbURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "imgURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136216/L2XGJI1LM.mp4?source=firetv&channel_id=13672",
      "categories" : [
         "The Country Jamaica"
      ],
      "channel_id" : "13672"
   }
]
```

Here's what this query syntax retrieves:

<table>
<colgroup>
   <col width="40%" />
   <col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>Query Syntax</th>
      <th>What It Matches</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>$[</code></td>
      <td>Starts at the root and selects the unnamed array.</td>
    </tr>
    <tr>
      <td><code>?(@.categories[0] in ["The Country Jamaica"])]</code></td>
      <td>Creates a filter for all <code>categories</code> arrays that contain <code>["The Country Jamaica"]</code> in the <code>0</code> index position.</td>
    </tr>
  </tbody>
</table>

Now there's one difference in query syntax used in the sample Fire App Builder app. Instead of `["The Country Jamaica"]`, the `query` parameter uses `$$par0$$` instead:

```
"query": "$[?(@.categories[0] in [$$par0$$])]"
```

`$$par0$$` is a variable defined in Fire App Builder that stores all the items retrieved by the query, and this is where the syntax differs from Jayway JsonPath. In the code, the `keyDataType` from the categories recipe actually populates the `$par0$$` variable in the contents recipe with categories.

Let's go through one more example. Suppose your JSON looks like this:

```json
{
    "titles": {
        "video": [
            {
                "category": "reference",
                "publisher": "Jess",
                "title": "Video title 1",
                "price": 3.95
            },
            {
                "category": "history",
                "publisher": "John",
                "title": "Video title 2",
                "price": 2.99
            },
            {
                "publisher": "Dave",
                "title": "Video title 3",
                "price": 8.99
            },
            {
                "category": "science",
                "publisher": "Jess",
                "title": "Video title 4",
                "price": 3.99
            }
        ]
     }
}
```

To select all arrays containing `science` as category, you would use this query:

```
$.titles.video[?(@.category contains "science")]
```

This returns:

```
[
   {
      "category" : "science",
      "publisher" : "Jess",
      "title" : "Video title 4",
      "price" : 3.99
   }
]
```

However, we need *all* items in the array that contain `category`, so we remove the conditions around the `@.category`:

```
$.titles.video[?(@.category)]
```

Now add the `in [$$par0$$]` variable:

```
$.titles.video[?(@.category in [$$par0$$])]
```

Here's a summary of the syntax:

<table>
<colgroup>
   <col width="40%" />
   <col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>Query Syntax</th>
      <th>What It Matches</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>$.titles.video[</code></td>
      <td>Selects the <code>title</code> object and the <code>video</code> array.</td>
    </tr>
    <tr>
      <td><code>?(@.category in [$$par0$$]</code></td>
      <td>Filters the array to match on all items that have a <code>category</code>.</td>
    </tr>
  </tbody>
</table>

Note that when you add in `[$$par0$$]` in the Jayway JsonPath Evaluator, the Evaluator will not understand this syntax because it's specific to Fire App Builder rather than being part of Jayway JsonPath.

Your query will look different based on your feed and the syntax necessary to match the content objects. For example, here's a more complex query:

```
$.assets[?(@.type == 'movie.Container' && @.assetId in $$par0$$)]
```

This query says to start at the root (`$`), look in the first directory level (`.`) to the object named `assets`, and filter the array to `type` objects that are equal to `movie.Container` and which contain an `assetId` element. Jayway JsonPath is powerful &mdash; you can use it to target the elements of almost any feed structure.

#### XML Feeds {#queryxml}

If your feed is XML, instead of using Jayway JsonPath, you will use [XPath expressions](https://www.w3schools.com/xml/xpath_syntax.asp) to target the specific elements in your feed. XPath reduces your XML document into various items called "nodes." The XPath syntax allows you to target the location of specific nodes.

In the [sample MRSS XML app][fire-app-builder-configure-mrss-feed], the value for the `query` parameter in the contents recipe is as follows:

```json
"query": "//item[./category='$$par0$$']"
```

Let's take a step back to unpack this syntax with more clarity, because you'll need to customize this query to fit your own feed syntax.

A sample XML feed looks like this:

```xml
<rss>
    <channel>
        <item>
            <title>Sample Title 1</title>
            <pubDate>Wed, 26 Oct 2016 20:34:22 PDT</pubDate>
            <link>https://example.com/myshow/episodes/110</link>
            <author>Sample Author name</author>
            <category>Gadgets</category>
        </item>

        <item>
            <title>Sample Title 2</title>
            <pubDate>Mon, 24 Oct 2016 09:24:12 PDT</pubDate>
            <link>https://example.com/myshow/episodes/109</link>
            <author>Sample Author name</author>
            <category>Technology</category>
        </item>
    </channel>
</rss>
```

With this feed structure, the `query` for your content recipe would be as follows:

```
//item[./category='$$par0$$']
```

Plug this sample XML feed into the [XPath Tester](http://www.freeformatter.com/xpath-tester.html). Then run the following query:

```
//item[./category='Technology']
```

The result is as follows:

```
Element='<item>
        <title>Sample Title 2</title>
        <pubDate>Mon, 24 Oct 2016 09:24:12 PDT</pubDate>
        <link>https://example.com/myshow/episodes/109</link>
        <author>Sample Author name</author>
        <category>Technology</category>
        </item>'
```

Here's what this query syntax retrieves:

<table>
<colgroup>
   <col width="40%" />
   <col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>Query Syntax</th>
      <th>What It Matches</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>//item</code></td>
      <td>Match all instances of the <code>item</code> element regardless of its position in the XML structure. </td>
    </tr>
    <tr>
      <td><code>?[./category='Technology']</code></td>
      <td>Selects all <code>categories</code> that are attributes of the <code>item</code> element where the category is equal to <code>Technology</code>.</td>
    </tr>
  </tbody>
</table>

Now there’s one difference in query syntax used in the sample Fire App Builder app. Instead of `["Technology"]`, the query parameter uses `$$par0$$` instead:

```json
"query": "//item[./category='$$par0$$']"
```

`$$par0$$` is a variable defined in Fire App Builder that stores all the items retrieved by the query, and this is where the syntax differs from XPath expressions. In the code, the `keyDataType` from the categories recipe actually populates the `$par0$$` variable in the contents recipe with categories.

After you confirm that your query targets a specific category of items in your feed, replace your category (e.g., `Technology`) with the variable `$$par0$$`.

Here's a summary of the syntax:

<table>
<colgroup>
   <col width="40%" />
   <col width="60%" />
</colgroup>
  <thead>
    <tr>
      <th>Query Syntax</th>
      <th>What It Matches</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>//item</code></td>
      <td>Selects all the <code>item</code> elements in the feed.</td>
    </tr>
    <tr>
      <td><code>[./category='$$par0$$']</code></td>
      <td>Filters the selection to match on all items that have a <code>category</code>.</td>
    </tr>
  </tbody>
</table>

See [Querying XML][fire-app-builder-querying-xml] for more details about constructing XPath queries.

### matchList Parameter {#matchlistparameter}

The `matchList` parameter selects specific properties in the query's result and maps the properties to Fire App Builder's content model. The syntax used by `matchList` is custom Fire App Builder syntax that targets specific elements.

In the sample app in Fire App Builder, the value for the Contents recipe is an array of property mappings that maps the title, id, description, media, and images to Fire App Builder's content model. This is what the `matchlist` parameter looks like in the sample app:

```json
[
    "title@title",
    "id@id",
    "description@description",
    "videoURL@url",
    "imgURL@cardImageUrl",
    "imgURL@backgroundImageUrl"
  ]
```

Here's how this syntax works. On the left of the ampersand (`@`) you put the property you want to target in the query result (for example, `title`). On the right of the ampersand (`@`), you put the Fire App Builder element you want to map the property to (for example, `title`).

The following table lists the Fire App Builder elements that you can map properties or elements of your feed to:

<table class="grid">
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>Fire App Builder Name</th>
<th>Description</th>
<th>Required</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="1">`title`
</td>
<td markdown="1">Video title.
</td>
<td markdown="1">
Required
</td>
</tr>

<tr>
<td markdown="1">`id`
</td>
<td markdown="1"> Video ID (string). Used to uniquely identify the media object.
</td>
<td markdown="1">
Required
</td>
</tr>

<tr>
<td markdown="1">`subtitle`
</td>
<td markdown="1">Subtitle for a video.
</td>
<td markdown="1">
Optional
</td>
</tr>

<tr>
<td markdown="1">`description`
</td>
<td markdown="1">Description of the video.
</td>
<td markdown="1">
Required
</td>
</tr>

<tr>
<td markdown="1">`url`
</td>
<td markdown="1">Link to the video.
</td>
<td markdown="1">
Required
</td>
</tr>

<tr>
<td markdown="1">`cardImageUrl`
</td>
<td markdown="1">Image thumbnail that appears on the home screen alongside other media thumbnails. The ideal size is 548px by 452px. See the following section on [Image Resolutions](#imageresolution) for more detail.
</td>
<td markdown="1">
Required
</td>
</tr>

<tr>
<td markdown="1">`backgroundImageUrl`
</td>
<td markdown="1">Larger image that appears when media is selected. The ideal size is 1920px x 1080px. See the following section on [Image Resolutions](#imageresolution) for more detail.
</td>
<td markdown="1">
Required
</td>
</tr>

<tr>
<td markdown="1">`tags`
</td>
<td markdown="1">Used to associate related content. See [Related Content (Through Tags)](#tags) for more information.
</td>
<td markdown="1">
Optional
</td>
</tr>

<tr>
<td markdown="1">`live`
</td>
<td markdown="1">Used to identify live stream content. For live stream content, the "Watch from Beginning" button is not shown when users return to the media after having previously watching it. See [Configure Live Streams][fire-app-builder-live-stream-configuration] for more information.
</td>
<td markdown="1">
Optional
</td>
</tr>
</tbody>
</table>

{% include warning.html content="The Fire App Builder content model *requires* you (at the very least) to map your feed's elements to the title, ID, description, URL, card image, and background image. If you have just one image, you can map both the card and background image to the same image." %}

To better understand how the `matchlist` parameter works, let's step through the details in the Fire App Builder sample app. In the LightCast media feed for Fire App Builder, the selections from the `query` parameter returns the following:

```json
[
   {
      "id" : "136211",
      "title" : "Legends Beach Resort, Negril Jamaica",
      "description" : "Legends Beach Resort, Negril Jamaica",
      "duration" : "538",
      "thumbURL" : "http://l3.cdn01.net/_thumbs/0000136/0136211/0136211__009f.jpg",
      "imgURL" : "http://l3.cdn01.net/_thumbs/0000136/0136211/0136211__009f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136211/M3FA8J1MI.mp4?source=firetv&channel_id=13671",
      "categories" : [
         "Jamaican Attractions"
      ],
      "channel_id" : "13671"
   },
   {
      "id" : "136216",
      "title" : "Falmouth Jamaica Nature's Lullaby ",
      "description" : "Falmouth Jamaica Nature's Lullaby",
      "duration" : "1813",
      "thumbURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "imgURL" : "http://l4.cdn01.net/_thumbs/0000136/0136216/0136216__001f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136216/L2XGJI1LM.mp4?source=firetv&channel_id=13672",
      "categories" : [
         "The Country Jamaica"
      ],
      "channel_id" : "13672"
   },
   {
      "id" : "136209",
      "title" : "Rafting on the Rio Grande River, Jamaica",
      "description" : "Rafting on the Rio Grande River, Jamaica",
      "duration" : "660",
      "thumbURL" : "http://l1.cdn01.net/_thumbs/0000136/0136209/0136209__010f.jpg",
      "imgURL" : "http://l1.cdn01.net/_thumbs/0000136/0136209/0136209__010f.jpg",
      "videoURL" : "http://media.cdn01.net/802E1F/process/encoded/video_1880k/0000136/0136209/I9BDFF0IM.mp4?source=firetv&channel_id=13671",
      "categories" : [
         "Jamaican Attractions"
      ],
      "channel_id" : "13671"
   }
]
```

The `matchList` parameter converts these property names in the feed to the names used in Fire App Builder's content model:

```json
[
    "title@title",
    "id@id",
    "description@description",
    "videoURL@url",
    "imgURL@cardImageUrl",
    "imgURL@backgroundImageUrl"
  ]
```

*  `title@title` converts `title` to `title`
*  `id@id` converts `id` to `id`
*  `description@description` converts `description` to `description`
*  `videoURL@url` converts `videoURL` to `url`
*  `imgURL@cardImageUrl` converts `imgURL` to `cardImageUrl`
*  `imgURL@backgroundImageUrl` converts `imgURL` to `backgroundImageUrl`

(In this example, a lot of the names are the same, but in your feed, might might have terms such as `videoTitle`, `videoSummary`, and so on.)

Customize the left side of the ampersand `@` to correspond with your own feed's terminology; customize the right side of the `@` to correspond with Fire App Builder's term in its content model.

Your feed might not list each element in a simple key-value pair. If you have various levels of nesting, your `matchList` might look like this:

```json
    "common/title@title",
    "assetId@id",
    "common/subtitle@subtitle",
    "common/description@description",
    "video/videoURL@url",
    "imgURLs/ImageSmall@cardImageUrl",
    "imgURLs/ImageLarge@backgroundImageUrl"
```

With this syntax, the forward slash (`/`) separates elements at each level. For example, `common/title` looks inside the `common` object to match on the `title` property.

Let's look an an example using XML. Suppose your XML feed looks like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<rss version="2.0" … …>
  <channel>
    <title>Content Mix for US News-in-Pictures 9x16</title>
    <description>Screenfeed Content Server</description>
    <lastBuildDate>Mon, 08 Dec 2014 22:55:16 GMT</lastBuildDate>
<ttl>5</ttl>

    <item>
      <title>John Anderson ……</title>
      <guid isPermaLink="false">1</guid>
      <pubDate>Mon, 08 Dec 2014 22:55:16 GMT</pubDate>
      <category>News</category>
      <media:content url="http://samples.screenfeed.com/1.jpg" type="image/jpeg">
        <media:title type="plain">1080x1920 - English - with caption</media:title>
        <media:credit>Â© 2014 News Agency</media:credit>
        <media:thumbnail url="http://samples.screenfeed.com/public/us-news-in-pictures/1080x1920/h9xnRIN9CUGiTWNQBBrjOw-1080x1920h-1.jpg" />
      </media:content>
</item>

...

  </channel>
</rss>
```

In order to get the contents, the query in the `matchList` parameter would look like this:

```json
  "matchList": [
    "title/#text@title",
    "guid/#text@id",
    "media:content/media:title/#text@description",
    "media:content/#attributes/url@url",
    "media:content/#attributes/url@cardImageUrl",
    "media:content/#attributes/url@backgroundImageUrl",
  ]
```

In this case, here's how the conversions happen:

*  `title/#text@mTitle` converts `title/#text` to `title`
*  `guid/#text@mId` converts `guid/#text` to `id`.  
*  `media:content/media:title/#text@mDescription` converts `media:content/media:title/#text` to `description`
*  `media:content/#attributes/url@mUr` converts `media:content/#attributes/url` to `url`
*  `media:content/#attributes/url@mCardImageUrl` converts `media:content/#attributes/url` to `cardImageUrl`
*  `media:content/#attributes/url@mBackgroundImageUrl` converts `media:content/#attributes/url` to `backgroundImageUrl`.

The only real difference here is the use of the `#text` selector, which targets the text inside an element. The `#text` syntax is custom Fire App Builder syntax and is equivalent to `text()` in XPath.

Here's another example. Suppose your feed looks like this:

```xml
<rss>
    <channel>
        <item>
        <title>Sample Title 1</title>
        <pubDate>Wed, 26 Oct 2016 20:34:22 PDT</pubDate>
        <link>https://example.com/myshow/episodes/110</link>
        <author>Sample Author name 1</author>
        <category>Technology</category>
        <category>Gadgets</category>
        </item>

        <item>
        <title>Sample Title 2</title>
        <pubDate>Wed, 23 Oct 2016 08:33:12 PDT</pubDate>
        <link>https://example.com/myshow/episodes/109</link>
        <author>Sample Author name 2</author>
        <category>News</category>
        </item>
    </channel>
</rss>
```

Here's what the `matchlist` parameter would look like:

```
"matchList": [
    "title/#text@title",
    "guid/#text@id",
    "itunes:subtitle/#text@description",
    "media:content/#attributes/url@url",
    "media:content/media:thumbnail/#attributes/url@cardImageUrl",
    "media:content/media:thumbnail/#attributes/url@backgroundImageUrl"
]
```

#### Image Resolutions {#imageresolution}

You can use two images for media in your app: an image card and a background image. The two types of images get used in different places, and the containers where each image is used also varies slightly.

The following screenshot shows the difference between the two types of images on the Content Home screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_home" type="png" caption="The image cards appear in a list of thumbnails. When you select one of the image cards, a larger background image appears in the upper-right area of the screen." %}

The two images also appear on the Content Preview screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_imagesdemoscreen2" type="png" caption="Here the image card appears a little larger, with the background image filling the entire background of the screen. The background image has a dark gray overlay." %}

The recommended image sizes (width x height) are as follows:

* **Image cards**: 548px by 452px. This image can be larger but will be scaled down. The image will also be cropped to fill a 320px x 240px container in some cases.
* **Background images**: 1920px x 1080px. This image can be larger but will be scaled down. The image will also be cropped to fill a 1120px x 800px container in some cases.

When Fire App Builder does image cropping, it preserves the aspect ratio of the image by cropping the sides of the image (thus focusing on the center).

#### Related Content (Through Tags) {#tags}

Below the video is a Related Content section that shows other videos with the same tags:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_reccontent" type="png" alt="Related content" %}

To populate the [Related Content section in the app][fire-app-builder-customize-look-and-feel#relatedcontent], you need to match your tags in the `matchList` parameter. For example:

```
common/tags@tags
```

Here the `tags` element appears inside a `common` element. This syntax converts `common/tags` to `tags` so Fire App Builder can read it and display related media objects.

Note that the LightCast feed in the Fire App Builder sample app doesn't include tags. However, there's a fallback parameter that you can set to `true` if you want to show related content, but you don't have tags in your feed.

In your **Navigator.json** file (located in your app's **assets** folder), the `config` object has a property called `useCategoryAsDefaultRelatedContent`:

```json
  "config": {
    "showRelatedContent": true,
    "useCategoryAsDefaultRelatedContent": true,
    "searchAlgo": "basic"
  }
```

If you set `useCategoryAsDefaultRelatedContent` to `true`, Fire App Builder will use other media assets from the same category for the related content (rather than pulling content with the same tags).

{% include note.html content="Currently, Related Content matches (which are based on tags in the feed) will match unlimited content if many items have the same tags. This is a known limitation." %}

To turn off the Related Content section, you can set `showRelatedContent` to false in **Navigator.json**.

## Next Steps

Now that you've configured the categories and contents for your app's media feed, you need to associate the feed with the app's UI. See [Navigator Configuration Overview][fire-app-builder-configure-navigator].

{% include links.html %}
