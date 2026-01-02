# Prompt: Personal Knowledge Base Category System

Create a comprehensive personal knowledge base category system based on Wikipedia's 13 main categories for organizing bookmarks and personal files. The system will be implemented as a hierarchical markdown structure suitable for GitHub Pages.

## Structure Requirements

* Use Header 2 (##) for the 13 main categories exactly as they appear on Wikipedia
* Use Header 3 (###) for subcategories in alphabetical order
* Use asterisks (*) for sub-subcategories as bulleted lists in alphabetical order
* Maximum depth: 3 levels (Headers 2, 3, and bulleted items only)
* Headers 2 and 3 should match Wikipedia's structure closely
* All formatting must be GitHub Flavored Markdown compatible
* No horizontal lines (---) anywhere in the document
* Use asterisks (*) for all bullet points, not hyphens (-)

## Content Requirements

1. **Title**: "# Personal Knowledge Base Categories"

2. **Introduction**: Brief paragraph explaining the system (2-3 sentences)

3. **Table of Contents**: Numbered list linking to all 13 main categories

4. **For Each Main Category**:
   * Header 2 with category name
   * **Wikipedia:** link to the corresponding Wikipedia category page (format: https://en.wikipedia.org/wiki/Wikipedia:Contents/Categories#[anchor])
   * **Description**: One sentence describing what belongs in this category
   * Header 3 subcategories in alphabetical order
   * Asterisk (*) bulleted sub-subcategories in alphabetical order under relevant subcategories

## The 13 Main Categories (from Wikipedia)

1. General Reference
2. Culture and the Arts
3. Geography and Places
4. Health and Fitness
5. History and Events
6. Human Activities
7. Mathematics and Logic
8. Natural and Physical Sciences
9. People and Self
10. Philosophy and Thinking
11. Religion and Belief Systems
12. Society and Social Sciences
13. Technology and Applied Sciences

## Reference Source

Base the structure on: https://en.wikipedia.org/wiki/Wikipedia:Contents/Categories

## Style Guidelines

* Keep descriptions concise (one sentence per category)
* Use clear, professional language
* Focus on practical categorization for personal use
* Ensure alphabetical ordering is strict for subcategories and sub-subcategories
* No implementation guides or how-to sections
* No examples of specific bookmarks or tools
* No tagging conventions or organizational tips

## Output Format

Deliver a single, clean markdown file with:
* Title and introduction
* Table of contents
* All 13 categories with their hierarchical structure
* Wikipedia links for each main category
* Consistent formatting throughout
* No extraneous content beyond the category structure itself

## Example Format

```markdown
# Personal Knowledge Base Categories

[Introduction paragraph]

## Table of Contents

1. [General Reference](#general-reference)
2. [Culture and the Arts](#culture-and-the-arts)
...

## General Reference

**Wikipedia:** https://en.wikipedia.org/wiki/Wikipedia:Contents/Categories#Reference

**Description**: Digital reference library for encyclopedias, dictionaries, databases, archives, and knowledge management tools.

### Academic Disciplines

### Archives and Databases
* Digital archives
* Historical records
* Patent databases
* Searchable databases

### Information and Knowledge Management
* Information architecture
* Note-taking methodologies
* Personal knowledge management systems
```

## Constraints

* Stick closely to Wikipedia's hierarchical structure
* Keep maximum depth at 3 levels
* No horizontal dividers
* Use asterisks for all bullets
* Alphabetize all subcategories and sub-subcategories
* Include Wikipedia link for each main category
* One-sentence descriptions only
* No implementation details or usage guidance
