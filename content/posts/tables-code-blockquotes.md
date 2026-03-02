---
title: "Tables, Code Blocks & Blockquotes: Markdown for Scholarly Writing"
date: 2026-02-15
slug: "tables-code-blockquotes"
description: "How Markdown's structured elements — tables, fenced code blocks with syntax highlighting, and blockquotes — serve the specific needs of Digital Humanities scholarship."
tags: ["Scholarship", "Code", "Tables"]
image: "https://picsum.photos/seed/scholarly/1920/1080"
---

Digital Humanities scholarship often sits at the intersection of humanistic interpretation and computational precision. This creates a unique set of publishing requirements: the ability to typeset prose and footnotes _alongside_ data tables, code samples, and archival quotations — often in the same document.

Markdown handles all of this natively.

---

## Data Tables

GFM (GitHub Flavored Markdown) tables use a simple pipe syntax that is both human-readable as plain text and renders into polished, styled HTML:

### Project Comparison Table

| Platform                | Monthly Cost | Technical Overhead |   Version History    | Deployment Time |
| :---------------------- | :----------: | :----------------: | :------------------: | --------------: |
| WordPress (self-hosted) |   ~$30–120   |        High        |   Plugin required    |    Manual/hours |
| WordPress (managed)     |   ~$50–300   |       Medium       |       Limited        |  Manual/minutes |
| **Hugo + GitHub Pages** |    **$0**    |    **Minimal**     | **Full Git history** | **~60 seconds** |
| Drupal (self-hosted)    |   ~$20–80    |     Very High      |   Plugin required    |    Manual/hours |
| Ghost (Pro)             |   ~$36–199   |        Low         |         None         |       Automatic |

### Corpus Statistics

| Corpus                     | Documents | Tokens | Date Range        | Access      |
| -------------------------- | --------: | -----: | ----------------- | ----------- |
| Early English Books Online |   125,000 |   900M | 1475–1700         | Licensed    |
| JSTOR Data for Research    |      12M+ |      — | 1665–present      | Application |
| Project Gutenberg          |   70,000+ |      — | to 1928           | Open        |
| HathiTrust Research Center |      17M+ |      — | 1700s–present     | Licensed    |
| Internet Archive           |      35M+ |      — | Antiquity–present | Open        |

{{< callout type="info" >}}
Tables in Markdown are left-aligned by default. Use `:---` for left, `:---:` for center, and `---:` for right column alignment.
{{< /callout >}}

---

## Fenced Code Blocks

Fenced code blocks — created with triple backticks and a language identifier — receive full syntax highlighting via Hugo's built-in Chroma highlighter (Dracula theme):

### Python: Text Analysis

```python
import collections
from pathlib import Path

def word_frequency(filepath: str, top_n: int = 20) -> dict:
    """
    Compute the top-N most frequent words in a text file.
    Suitable for basic corpus analysis in a DH research workflow.
    """
    text = Path(filepath).read_text(encoding="utf-8").lower()

    # Remove punctuation and tokenize
    tokens = [
        word.strip(".,;:!?\"'()[]")
        for word in text.split()
        if len(word) > 3  # skip stop words by length (naive)
    ]

    counter = collections.Counter(tokens)
    return dict(counter.most_common(top_n))

results = word_frequency("corpus/hamlet.txt")
for word, count in results.items():
    print(f"{word:<20} {count:>6}")
```

### R: Corpus Visualization

```r
library(tidyverse)
library(tidytext)

# Load and tokenize a set of humanities texts
corpus <- read_csv("data/texts.csv") |>
  unnest_tokens(word, text) |>
  anti_join(stop_words) |>
  count(author, word, sort = TRUE)

# Plot top terms per author
corpus |>
  group_by(author) |>
  slice_max(n, n = 15) |>
  ungroup() |>
  ggplot(aes(x = reorder_within(word, n, author), y = n, fill = author)) +
    geom_col(show.legend = FALSE) +
    facet_wrap(~author, scales = "free") +
    scale_x_reordered() +
    coord_flip() +
    labs(title = "Top Terms by Author", x = NULL, y = "Frequency") +
    theme_minimal(base_size = 14)
```

### Shell: Hugo Build & Deploy

```bash
#!/usr/bin/env bash
# Local development workflow

# Install Hugo (macOS)
brew install hugo

# Serve locally with live reload
hugo server --buildDrafts --navigateToChanged

# Build for production
hugo --minify --gc

# Check the output
ls -lh public/
```

### YAML: Frontmatter Structure

```yaml
---
title: "Digital Archives and Computational Analysis"
date: 2026-02-26
author: "Your Name"
description: "A study of 18th-century pamphlet literature using NLP."
tags:
  - "text-analysis"
  - "18th-century"
  - "pamphlets"
series: "Computational Approaches to British Literature"
draft: false
---
```

### JSON-LD: Structured Data for Scholarship

```json
{
  "@context": "https://schema.org",
  "@type": "ScholarlyArticle",
  "headline": "Distant Reading the Early Modern Archive",
  "author": {
    "@type": "Person",
    "name": "Jane Scholar",
    "affiliation": {
      "@type": "Organization",
      "name": "Digital Humanities Department"
    }
  },
  "datePublished": "2026-02-26",
  "keywords": ["distant reading", "Early Modern", "text analysis"],
  "isPartOf": {
    "@type": "Periodical",
    "name": "DH Illuminate Journal"
  }
}
```

---

## Blockquotes in Academic Writing

Blockquotes in scholarly publishing carry significant semantic weight — they signal direct citation, archival transcription, or literary quotation. DH Illuminate styles them distinctively to set them apart from prose:

### Simple Citation

> "Computation opens the field, but it is the humanist who must ultimately close the interpretation."
> <cite>— Panel discussion, DH2023, Graz</cite>

### Extended Archival Transcription

> Memorandum, Lord Privy Seal to the Committee of the Whole House: _That whereas diverse and sundry persons have this last winter season past used to carry and transport out of this realm divers and great sums of money, jewels, plate, and other things, to the great impoverishment of the realm..._ the said practice is henceforth strictly forbidden under pain of forfeiture.
> <cite>— State Papers, Domestic Series, 1547. TNA SP 10/1/43</cite>

### Nested Scholarly Commentary

> Moretti's argument rests on a fundamental provocation:
>
> > "Instead of close reading, I will suggest distant reading: where distance is not an obstacle, but a specific form of knowledge — fewer elements, hence a sharper sense of their overall interconnection."
>
> This methodological inversion — treating distance as _productive_ rather than limiting — became the founding gesture of computational literary studies.
> <cite>— Summary of Franco Moretti, _Distant Reading_ (2013)</cite>

---

## Combining Elements: A Research Note

The true power of Markdown for scholarship is combining all these elements in a single document. Here is a mock research note in DH style:

### Research Note: Token Frequencies in Parliamentary Debates, 1803–1820

**Corpus:** _Hansard Parliamentary Debates_, Vol. 1–41 (Project Gutenberg digitization)
**Method:** Bag-of-words frequency analysis with stop-word removal
**Tools:** Python 3.12 / NLTK / pandas

**Top 10 non-stop tokens (lemmatized):**

| Rank | Token      | Raw Frequency | Normalized (per 100k) |
| ---: | ---------- | ------------: | --------------------: |
|    1 | honourable |        84,221 |                 412.3 |
|    2 | gentleman  |        61,904 |                 303.0 |
|    3 | house      |        58,332 |                 285.5 |
|    4 | minister   |        41,109 |                 201.2 |
|    5 | war        |        38,445 |                 188.1 |
|    6 | ireland    |        35,200 |                 172.3 |
|    7 | majesty    |        28,774 |                 140.8 |
|    8 | france     |        26,611 |                 130.2 |
|    9 | navy       |        19,088 |                  93.4 |
|   10 | commerce   |        17,403 |                  85.2 |

**Preliminary observation:**

> The dominance of procedural vocabulary ("honourable," "gentleman," "house") in early 19th-century debates aligns with prior findings on Hansard rhetoric. The co-presence of "war," "ireland," and "france" in the top 10 is consistent with the Napoleonic Wars period (1803–1815) and the Act of Union debates.

```python
# Reproduce this analysis:
import pandas as pd
hansard = pd.read_parquet("data/hansard_1803_1820.parquet")
tokens = hansard["speech_text"].str.lower().str.split().explode()
freq = tokens.value_counts().head(10)
print(freq)
```

{{< callout type="warning" >}}
This is illustrative data. Always validate token counts against the raw corpus before citing in publication.
{{< /callout >}}
