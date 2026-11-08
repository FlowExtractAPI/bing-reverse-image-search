**[Bing Reverse Image Search](https://apify.com/dz_omar/bing-reverse-image-search?fpr=smcx63)** takes a picture — an image link or a file you upload — and returns what **Bing** finds for it: Bing's **best guess** of what the picture shows, **hundreds of visually similar images** (full-size URL, size, file type, publish date), the **pages that carry the picture**, **related and shopping searches**, any **text written in the picture**, and an **AI description** with its sources.

Perfect for **photographers and brands** tracing where their images are reused, **e-commerce and marketplace teams** checking product photos, **journalists and researchers** verifying where a picture came from, and **SEO teams** looking for backlink and image-credit opportunities — without a Bing account or an API key.

## What does Bing Reverse Image Search do?

Give it one picture or a list of hundreds. For each one it returns a summary row and then the results you choose, ready for JSON, CSV or Excel.

- **Bing's best guess** — the name Bing gives the picture, plus the search it suggests for it.
- **Visually similar images** — most similar first, each with the full-size image URL, a thumbnail, Bing's own cached copy (still works when the original is taken down), width and height, file size, file type, publish date and the page it sits on. A single picture can return several hundred.
- **Pages that carry the picture** — the web pages where the picture itself appears, with their titles and dates.
- **Related and shopping searches** — what people search for with this picture, ready to feed keyword research.
- **Text in the picture** — every line of writing Bing can read, with where it sits on the picture.
- **AI description** — Bing's short description of the picture with numbered sources.
- **Links, files and data links** — paste image links, upload a file, or pass a `data:` link. Large pictures are resized automatically; you never have to prepare them.
- **Region and safety** — 18 Bing markets and all three SafeSearch levels.

### What data can you extract from Bing Reverse Image Search?

| Result type | Fields |
|---|---|
| Picture summary (`image_summary`) | `bestGuess`, `suggestedQuery`, `bestRepresentativeQuery`, `hasText`, `similarImagesAvailable`, the counts returned, `textInImage`, and the size and type of the picture you supplied |
| Similar image (`similar_image`) | `title`, `link`, `source`, `original`, `originalWidth`, `originalHeight`, `thumbnail`, `cachedImage`, `fileSize`, `format`, `datePublished`, `dateDiscovered`, `pagesIncludingCount`, `availableSizesCount`, `accentColor`, `imageId`, `bingUrl` |
| Page that carries the picture (`matching_page`) | the same fields as a similar image |
| Related search (`related_search`) | `text`, `category` (`related` or `shopping`), `searchUrl`, `thumbnail` |
| Text in the picture (`text`) | `text`, `boundingBox` (the line's four corners as fractions of the picture) |
| AI description (`ai_overview`) | `text`, `citations` (number, URL, title, source, snippet) |

Values are delivered as Bing gives them: dates keep Bing's format and nothing is rounded or translated. Only fields that carry no information for you (tracking tokens) are left out, and a value Bing did not send is `null` rather than guessed. Every row also carries `searchImage` (the picture it belongs to), `scrapedAt` and `_source`.

### Why use Bing Reverse Image Search on Apify?

Running it on Apify gives you **API access**, **scheduling**, **monitoring**, **integrations** (Make, Zapier, n8n, webhooks) and **proxy rotation** without managing infrastructure. One picture with the default settings finishes in about ten seconds; a picture with several hundred similar images takes under a minute.

## How to run a reverse image search on Bing

1. Open Bing Reverse Image Search in Apify Console.
2. Paste one or more image links into **Image links**, or upload a picture file. Use the direct address of the image — right-click the picture and choose *Copy image address*.
3. Choose **What to return**. Similar images, pages and related searches are on by default.
4. Set **Max results per type**. Start small: `50`.
5. Click **Start** and wait for the run to finish.
6. Download the results as JSON, CSV or Excel, or fetch them over the API.

## How much does it cost to run a reverse image search on Bing?

You pay for what you receive, in two parts. A picture is charged once, and only when Bing returns something for it; each similar image or page that carries the picture is charged as a match.

| Event | FREE | BRONZE | SILVER | GOLD |
|---|---|---|---|---|
| Picture searched (once per picture; includes the summary, related and shopping searches, text in the picture and the AI description) | $0.0040 | $0.0030 | $0.0025 | $0.0020 |
| Match returned (each similar image or page that carries the picture) | $0.0010 | $0.0008 | $0.0006 | $0.0005 |

- One picture with the defaults (up to 50 similar images and 50 pages) costs about **$0.10** at the highest tier price and **$0.05** on GOLD.
- A picture that cannot be loaded, that Bing returns nothing for, or that fails, is **never charged**. Notices are never charged.
- A match is never delivered or charged twice for a picture.
- Test with a low **Max results per type** first, and set a maximum cost per run in Apify — the Actor sizes the run to fit it.

## Input and output examples

### Input

```json
{
    "imageUrls": ["https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png"],
    "resultTypes": ["similarImages", "pagesWithImage", "relatedSearches", "textInImage", "aiOverview"],
    "maxResults": 50,
    "market": "us",
    "safeSearch": "moderate"
}
```

| Input value | What it does |
|---|---|
| `imageUrls` | Image links, one picture each. Duplicates are removed. |
| `imageFile` | A picture uploaded from your computer, searched in addition to the links. |
| `resultTypes` | Which results to return: `similarImages`, `pagesWithImage`, `relatedSearches`, `textInImage`, `aiOverview`. |
| `maxResults` | Rows per picture **and per result type** (not a global total). `0` returns everything Bing has. |
| `market`, `safeSearch` | The country and language Bing answers in, and how strictly it filters adult images. |

### Output

Every row is labelled with a `resultType`. Real results for a photo of the Eiffel Tower (long addresses shortened):

```json
[
    {
        "resultType": "image_summary",
        "status": "ok",
        "bestGuess": "Eiffel Tower",
        "suggestedQuery": "Eiffel Tower",
        "bestRepresentativeQuery": "the eiffel tower ________ paris; france",
        "hasText": false,
        "inputWidth": 2900,
        "inputHeight": 4830,
        "inputFormat": "jpeg",
        "similarImagesAvailable": 497,
        "similarImagesReturned": 50,
        "pagesReturned": 50,
        "relatedSearchesReturned": 49,
        "market": "us",
        "searchImage": "https://upload.wikimedia.org/wikipedia/commons/8/85/Tour_Eiffel_Wikimedia_Commons_%28cropped%29.jpg",
        "scrapedAt": "2026-11-08T19:04:20Z"
    },
    {
        "resultType": "similar_image",
        "position": 1,
        "title": "Eiffel Tower 5th Grade Quiz | Quizizz",
        "link": "https://quizizz.com/admin/quiz/.../eiffel-tower",
        "source": "quizizz.com",
        "original": "https://media.quizizz.com/_mdserver/main/media/resource/.../...-v2",
        "originalWidth": 800,
        "originalHeight": 1332,
        "thumbnail": "https://tse3.mm.bing.net/th/id/OIP.etoF3uybCDUssav9b7-ZHQHaMV?r=0&pid=Api",
        "cachedImage": "https://th.bing.com/th/id/R.7ada05deec9b08352cb1abfd6fbf991d?...",
        "fileSize": 179272,
        "format": "jpeg",
        "datePublished": "2025-06-14T03:37:00.0000000Z",
        "dateDiscovered": "2025-06-14T00:00:00.0000000Z",
        "pagesIncludingCount": 171,
        "availableSizesCount": 62,
        "accentColor": "244167",
        "imageId": "2C92A4688465FA063AF14E1D4DAC449BE89F56B9"
    },
    {
        "resultType": "matching_page",
        "position": 1,
        "title": "Torre Eiffel | Torre eiffel, Monumento, Parigi",
        "link": "https://www.pinterest.com/pin/52565520644350730/",
        "source": "pinterest.com",
        "original": "https://i.pinimg.com/736x/47/53/3c/47533c148d0318f8f821cd82d68082cb.jpg",
        "originalWidth": 736,
        "originalHeight": 1308,
        "fileSize": 112350,
        "format": "jpeg",
        "datePublished": "2024-12-30T11:50:00.0000000Z"
    },
    {
        "resultType": "related_search",
        "position": 1,
        "text": "Eiffel Tower Restaurant Paris France",
        "category": "related",
        "searchUrl": "https://www.bing.com/images/search?q=Eiffel+Tower+Restaurant+Paris+France&FORM=IDINTS"
    }
]
```

(Each row above also carries the fields listed in the table; a few are omitted here for length.)

## Other Actors you might need

| Actor | What it adds |
|---|---|
| **[Yandex Reverse Image Search](https://apify.com/dz_omar/yandex-reverse-image-search?fpr=smcx63)** | The same kind of search on Yandex: pages that carry the picture, similar images, other sizes and text in the picture. |
| **[Bing Images Scraper](https://apify.com/dz_omar/bing-images-scraper?fpr=smcx63)** | Start from a keyword instead of a picture: thousands of Bing images per search, with filters. |

## Frequently asked questions

### Is it legal to run a reverse image search on Bing?

The Actor collects publicly available search results, the same ones anyone sees in a browser, and respects the source's limits. Image URLs point to content owned by other people, so check the license and your local rules (including GDPR and CCPA where personal data is visible in a picture) before reusing anything. This is not legal advice.

### Do I need a Bing or Microsoft account or an API key?

No. Bing Reverse Image Search works from a picture alone — no Bing or Microsoft account, no Azure key and no API subscription.

### Which pictures can I search with?

JPG, PNG, WebP, GIF (the first frame), BMP and TIFF, up to about 25 MB. Give a direct image link, upload a file, or pass a `data:` link. AVIF and HEIC are not supported; convert them first. A link to a web page rather than a picture is reported as a notice and never fails the run.

### Does it identify people in photos?

The Actor has no face recognition of its own. It returns what Bing returns for the picture — its best guess, similar images, the pages that carry it — and what Bing says about people in a photo is up to Bing.

### How many similar images can I get for one picture?

It depends on the picture. A rarely published photo may have a few dozen; a well-known landmark or product can have several hundred (a single photo returned more than 600 in testing). Set **Max results per type** to `0` to take everything Bing returns. The number Bing estimates is reported as `similarImagesAvailable`; it is an estimate and moves between runs.

### Why do two runs on the same picture give slightly different results?

Bing orders and trims its lists a little differently from one request to the next, and its best guess can change by a word. The Actor removes duplicates within a picture, but it cannot make Bing repeatable.

### Does it download the images?

No. It returns image URLs, Bing's cached copies, thumbnails, sizes and source pages. Download the files you need from those URLs.

### Does the market change the results?

Yes. The market sets the language Bing writes in and noticeably changes which pages, related searches and how many similar images it holds for a picture. Run the same picture in two markets to compare.

### Can I use Bing Reverse Image Search in Make, Zapier, n8n or my own code?

Yes. Use Apify's integrations, webhooks, the API or the client libraries for JavaScript and Python.

### What happens if a run is interrupted?

The Actor saves its progress and picks up where it left off, without repeating or re-charging rows it has already delivered.

### Why did I get fewer results than I asked for, or a notice instead?

| Situation | What you see | What to do |
|---|---|---|
| Bing holds fewer matches for this picture | A shorter list; `similarImagesAvailable` shows Bing's estimate | Try another market, or a larger version of the picture |
| The link is a web page, a missing file, or not a picture | One notice row with `resultType: "guidance"` and a `status` such as `not_an_image` (not charged) | Use the direct image address, or upload the file |
| The picture is too small or too large | A notice with `too_small` or `too_large` (not charged) | Use a picture between about 16 pixels and 25 megapixels |
| Bing returned nothing for the picture | A summary row with `status: "no_matches"` (not charged) | Try a sharper, larger version |
| Bing declined the picture | A notice with `rejected` (not charged) | Try another version; if every picture is declined, run again later |
| Bing did not answer | A notice with `bing_unavailable` (not charged); the picture is kept for a resume | Run it again later |
| The run reached its time limit | A final `time_limit` notice | Raise the run timeout, or search fewer pictures per run |
| Your maximum cost per run was reached | The run stops after the last row it charged for | Raise the maximum cost per run |

## Support

- 🌐 **Website:** [flowextractapi.com](https://flowextractapi.com)
- 📧 **Email:** [flowextractapi@outlook.com](mailto:flowextractapi@outlook.com)
- 💬 **GitHub:** [FlowExtractAPI](https://github.com/FlowExtractAPI)
- 💼 **LinkedIn:** [flowextract-api](https://www.linkedin.com/in/flowextract-api/)
- 🐦 **X (Twitter):** [@FlowExtractAPI](https://x.com/FlowExtractAPI)
- 📱 **Facebook:** [flowextractapi](https://www.facebook.com/flowextractapi)
- 🎵 **TikTok:** [@flowextractapi](https://www.tiktok.com/@flowextractapi)

## Legal & compliance

- **Public data only** — processes publicly available data.
- **Rate limiting** — respects source-service limits and terms of use.
- **Data protection** — no storage of personal information or unauthorized data collection.
- **Commercial use** — suitable for business intelligence and research.
- No affiliation with or endorsement by Microsoft or Bing is implied.

---

*Bing Reverse Image Search — by FlowExtract API. Turn any website into structured data.*
