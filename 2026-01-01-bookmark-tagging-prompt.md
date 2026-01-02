# Bookmark Content-Based Tagging System

## Overview

This document provides a complete workflow for analyzing bookmarks, generating content-based tags, and updating them in Raindrop.io via the API.

---

## Part 1: Setup

### 1.1 Get Raindrop API Token

1. Go to https://app.raindrop.io/settings/integrations
2. Click "Create test token"
3. Copy the token and save it securely

### 1.2 Install Dependencies

```bash
pip install raindrop-io-py requests python-dotenv
```

### 1.3 Configure Environment

Create a `.env` file:
```
RAINDROP_TOKEN=your-api-token-here
```

---

## Part 2: Export Bookmarks from Raindrop API

This script exports all your bookmarks with their Raindrop IDs (needed for updating):

```python
#!/usr/bin/env python3
"""
export_bookmarks.py
Export all Raindrop bookmarks to JSON for processing
"""

import os
import json
from datetime import datetime
from dotenv import load_dotenv
from raindropiopy import API, Collection, CollectionRef, Raindrop

load_dotenv()

def get_all_collections(api):
    """Get all collections including system ones"""
    collections = {}
    
    # Add system collections
    collections[-1] = "Unsorted"
    collections[-99] = "Trash"
    
    # Get user collections
    for coll in Collection.get_collections(api):
        collections[coll.id] = coll.title
    
    return collections

def export_all_bookmarks(output_file="bookmarks_export.json"):
    """Export all bookmarks with their IDs"""
    
    with API(os.environ["RAINDROP_TOKEN"]) as api:
        collections = get_all_collections(api)
        
        all_bookmarks = []
        
        # Export from each collection
        for coll_id, coll_name in collections.items():
            print(f"Exporting: {coll_name}...")
            
            try:
                if coll_id == -1:
                    raindrops = list(Raindrop.search(api, collection=CollectionRef.Unsorted))
                elif coll_id == -99:
                    raindrops = list(Raindrop.search(api, collection=CollectionRef.Trash))
                else:
                    raindrops = list(Raindrop.search(api, collection=coll_id))
                
                for rd in raindrops:
                    all_bookmarks.append({
                        "id": rd.id,
                        "title": rd.title,
                        "url": rd.link,
                        "collection_id": coll_id,
                        "collection_name": coll_name,
                        "tags": rd.tags or [],
                        "created": rd.created.isoformat() if rd.created else None,
                        "domain": rd.domain,
                        "excerpt": rd.excerpt,
                    })
            except Exception as e:
                print(f"  Error: {e}")
        
        # Save to file
        with open(output_file, 'w', encoding='utf-8') as f:
            json.dump({
                "exported_at": datetime.now().isoformat(),
                "total_count": len(all_bookmarks),
                "bookmarks": all_bookmarks
            }, f, indent=2, ensure_ascii=False)
        
        print(f"\nExported {len(all_bookmarks)} bookmarks to {output_file}")

if __name__ == "__main__":
    export_all_bookmarks()
```

---

## Part 3: AI Tagging Prompt

Use this prompt with Claude (or another AI) to generate tags:

```
You are a bookmark librarian. Your task is to analyze bookmarks, fetch their content when possible, and assign descriptive tags based on what you learn.

## Input Format

I will provide bookmarks as JSON:
{
  "id": 12345,
  "title": "...",
  "url": "...",
  "collection_name": "...",
  "tags": [...],
  "excerpt": "..."
}

## Tagging Philosophy

Tags are simple descriptive words. Many will naturally match folder names from this Wikipedia-style hierarchy:

- 00-favorites
- 01-general-reference (archives, bookmarks, books, knowledge, lists, reading, reference, wikipedia, writing)
- 02-culture-and-the-arts (culture, films, games, humor, language, literature, marketing, media, music, visual-arts)
- 03-geography-places (travel, cities, countries, san-francisco, california)
- 04-health-and-fitness (aging, diet, fitness, health, medicine, psychology, wellness)
- 05-history-and-events (history, timelines, trends, futures)
- 06-human-activities (agriculture, business, communication, community, education, government, investing, politics, transport)
- 07-mathematics-logic (math, logic, category-theory)
- 08-natural-and-physical-sciences (biology, physics, science)
- 09-people-self (people, friends, family)
- 10-philosophy-and-thinking (philosophy, epistemology, ethics, thinking, mental-models)
- 11-religion-and-belief-systems (religion, bible)
- 12-society-and-social-sciences (anthropology, economics, law, linguistics, psychology, political-science)
- 13-technology-applied-sciences (architecture, cartography, computing, computing-ai, computing-software, electronics, engineering, hardware)

## Tag Rules

1. **Use plain descriptive words** — no namespaces, no prefixes
2. **Include publication date** — format: YYYY-MM-DD (or YYYY-MM or YYYY if partial)
3. **No limit on number of tags** — add as many as accurately apply
4. **Folder names are valid tags** — if content fits a folder category, use that name
5. **Be specific** — prefer "midjourney" over just "ai-image" when applicable
6. **Include notable authors** — add creator names as tags when relevant

## Instructions

For each bookmark:

1. **Fetch the URL** if publicly accessible
2. **Analyze the content** for subjects, topics, publication date, author
3. **Generate tags** as descriptive words plus date

## Output Format (CRITICAL)

Output as JSON array for direct API use:

```json
[
  {
    "id": 12345,
    "tags": ["computing-ai", "ai-agents", "benchmarks", "2024-07-01"]
  },
  {
    "id": 12346,
    "tags": ["health", "aging", "longevity", "research", "2023-11-15"]
  }
]
```

## Date Extraction

Look for dates in:
- Article bylines and timestamps
- URL patterns (/2024/07/15/)
- arXiv IDs (2407.01502 = July 2024 → 2024-07)
- GitHub release dates
- "Published" or "Updated" text

Format: YYYY-MM-DD (or YYYY-MM or YYYY if partial)

## Examples

Input:
{"id": 99001, "title": "karpathy/rendergit", "url": "https://github.com/karpathy/rendergit", "collection_name": "Unsorted", "tags": []}

Output:
[{"id": 99001, "tags": ["computing-software", "computing-ai", "github", "python", "tool", "llm", "karpathy", "2024-08"]}]

Input:
{"id": 99002, "title": "AI Agents That Matter", "url": "https://arxiv.org/abs/2407.01502", "collection_name": "computing-ai", "tags": []}

Output:
[{"id": 99002, "tags": ["computing-ai", "ai-agents", "benchmarks", "evaluation", "machine-learning", "research", "arvind-narayanan", "princeton", "2024-07-01"]}]

Input:
{"id": 99003, "title": "The Science of Loneliness", "url": "https://psychologytoday.com/...", "collection_name": "psychology", "tags": ["loneliness"]}

Output:
[{"id": 99003, "tags": ["loneliness", "psychology", "health", "wellness", "social-connection", "mental-health", "2024-03-12"]}]
```

---

## Part 4: Import Tags Back to Raindrop

This script reads the AI-generated JSON and updates Raindrop via API:

```python
#!/usr/bin/env python3
"""
import_tags.py
Update Raindrop bookmarks with AI-generated tags
"""

import os
import json
import time
from dotenv import load_dotenv
from raindropiopy import API, Raindrop

load_dotenv()

def update_bookmark_tags(api, bookmark_id, new_tags, merge=True):
    """
    Update tags for a single bookmark
    
    Args:
        api: Raindrop API instance
        bookmark_id: Raindrop ID
        new_tags: List of new tags
        merge: If True, merge with existing tags. If False, replace.
    """
    try:
        # Get existing bookmark
        existing = Raindrop.get(api, id=bookmark_id)
        
        if merge and existing.tags:
            # Merge tags, avoiding duplicates
            combined_tags = list(set(existing.tags + new_tags))
        else:
            combined_tags = new_tags
        
        # Update the bookmark
        Raindrop.update(api, id=bookmark_id, tags=combined_tags)
        return True, combined_tags
        
    except Exception as e:
        return False, str(e)

def import_tags_from_json(input_file="tagged_bookmarks.json", merge=True, dry_run=False):
    """
    Import tags from AI-generated JSON file
    
    Args:
        input_file: JSON file with format [{"id": 123, "tags": ["a", "b"]}, ...]
        merge: If True, merge new tags with existing. If False, replace.
        dry_run: If True, just print what would happen without making changes.
    """
    
    # Load the tagged bookmarks
    with open(input_file, 'r', encoding='utf-8') as f:
        tagged_bookmarks = json.load(f)
    
    print(f"Loaded {len(tagged_bookmarks)} bookmarks to update")
    
    if dry_run:
        print("\n=== DRY RUN MODE ===\n")
        for item in tagged_bookmarks[:10]:
            print(f"Would update ID {item['id']} with tags: {item['tags']}")
        if len(tagged_bookmarks) > 10:
            print(f"... and {len(tagged_bookmarks) - 10} more")
        return
    
    # Connect to API and update
    with API(os.environ["RAINDROP_TOKEN"]) as api:
        success_count = 0
        error_count = 0
        
        for i, item in enumerate(tagged_bookmarks):
            bookmark_id = item["id"]
            new_tags = item["tags"]
            
            success, result = update_bookmark_tags(api, bookmark_id, new_tags, merge)
            
            if success:
                success_count += 1
                print(f"[{i+1}/{len(tagged_bookmarks)}] Updated {bookmark_id}: {result}")
            else:
                error_count += 1
                print(f"[{i+1}/{len(tagged_bookmarks)}] ERROR {bookmark_id}: {result}")
            
            # Rate limiting - Raindrop allows 120 requests per minute
            if (i + 1) % 100 == 0:
                print("Pausing for rate limit...")
                time.sleep(10)
        
        print(f"\nComplete: {success_count} updated, {error_count} errors")

if __name__ == "__main__":
    import sys
    
    if len(sys.argv) > 1:
        input_file = sys.argv[1]
    else:
        input_file = "tagged_bookmarks.json"
    
    # First do a dry run
    print("=== DRY RUN ===")
    import_tags_from_json(input_file, merge=True, dry_run=True)
    
    # Ask for confirmation
    response = input("\nProceed with actual update? (yes/no): ")
    if response.lower() == "yes":
        import_tags_from_json(input_file, merge=True, dry_run=False)
    else:
        print("Aborted.")
```

---

## Part 5: Complete Workflow

### Step 1: Export
```bash
python export_bookmarks.py
# Creates: bookmarks_export.json
```

### Step 2: Prepare for AI
```python
# Convert to prompt-friendly batches
import json

with open("bookmarks_export.json") as f:
    data = json.load(f)

# Process in batches of 50
bookmarks = data["bookmarks"]
batch_size = 50

for i in range(0, len(bookmarks), batch_size):
    batch = bookmarks[i:i+batch_size]
    with open(f"batch_{i//batch_size + 1}.json", "w") as f:
        json.dump(batch, f, indent=2)
    print(f"Created batch_{i//batch_size + 1}.json with {len(batch)} bookmarks")
```

### Step 3: Process with AI

For each batch file:
1. Open Claude (or your preferred AI)
2. Paste the tagging prompt from Part 3
3. Paste the batch JSON
4. Copy the output JSON array
5. Save to `tagged_batch_N.json`

### Step 4: Merge Tagged Batches
```python
import json
import glob

all_tagged = []
for f in sorted(glob.glob("tagged_batch_*.json")):
    with open(f) as file:
        all_tagged.extend(json.load(file))

with open("tagged_bookmarks.json", "w") as f:
    json.dump(all_tagged, f, indent=2)

print(f"Combined {len(all_tagged)} tagged bookmarks")
```

### Step 5: Import to Raindrop
```bash
python import_tags.py tagged_bookmarks.json
```

---

## Part 6: Utility Scripts

### 6.1 Check Current Tags
```python
#!/usr/bin/env python3
"""List all tags currently in use"""

import os
from collections import Counter
from dotenv import load_dotenv
from raindropiopy import API, Tag

load_dotenv()

with API(os.environ["RAINDROP_TOKEN"]) as api:
    tags = Tag.get(api)
    print(f"Total unique tags: {len(tags)}\n")
    
    # Sort by count
    for tag in sorted(tags, key=lambda t: -t.count)[:50]:
        print(f"  {tag.tag}: {tag.count}")
```

### 6.2 Bulk Add Tag by Collection
```python
#!/usr/bin/env python3
"""Add a tag to all bookmarks in a collection"""

import os
import sys
from dotenv import load_dotenv
from raindropiopy import API, Raindrop

load_dotenv()

collection_id = int(sys.argv[1])  # e.g., 12345
tag_to_add = sys.argv[2]          # e.g., "computing-ai"

with API(os.environ["RAINDROP_TOKEN"]) as api:
    raindrops = list(Raindrop.search(api, collection=collection_id))
    print(f"Adding '{tag_to_add}' to {len(raindrops)} bookmarks...")
    
    for rd in raindrops:
        existing_tags = rd.tags or []
        if tag_to_add not in existing_tags:
            Raindrop.update(api, id=rd.id, tags=existing_tags + [tag_to_add])
            print(f"  Updated: {rd.title[:50]}")
```

### 6.3 Find Bookmarks Without Date Tags
```python
#!/usr/bin/env python3
"""Find bookmarks that don't have a date tag"""

import os
import re
from dotenv import load_dotenv
from raindropiopy import API, Raindrop, CollectionRef

load_dotenv()

date_pattern = re.compile(r'^\d{4}(-\d{2})?(-\d{2})?$')

def has_date_tag(tags):
    return any(date_pattern.match(t) for t in (tags or []))

with API(os.environ["RAINDROP_TOKEN"]) as api:
    # Check all bookmarks
    all_raindrops = list(Raindrop.search(api, collection=CollectionRef.All))
    
    missing_date = [rd for rd in all_raindrops if not has_date_tag(rd.tags)]
    
    print(f"Bookmarks without date tag: {len(missing_date)} / {len(all_raindrops)}")
    print("\nExamples:")
    for rd in missing_date[:20]:
        print(f"  [{rd.id}] {rd.title[:50]}")
        print(f"       Tags: {rd.tags}")
```

---

## Part 7: Notes

### Rate Limits
- Raindrop API: 120 requests/minute
- The import script includes automatic pausing

### Tag Merge vs Replace
- Default is **merge** — new tags are added to existing
- Set `merge=False` to replace all tags

### Handling Errors
- Failed updates are logged but don't stop the process
- Re-run the import; already-tagged items will just get duplicates removed

### Backup
- Always export before bulk operations
- Raindrop also has automatic backups in Settings

### Processing Order Priority
1. **Unsorted** (40) — needs triage
2. **GitHub repos** (122) — rich content, easy to fetch
3. **arXiv papers** (25) — dates in IDs
4. **Large untagged collections**
