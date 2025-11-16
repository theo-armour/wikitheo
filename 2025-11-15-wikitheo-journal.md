# WikiTheo Journal

## 2025-11-15 Prompt 02

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
* Blank line after all headers.

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

## 2025-11-15 01

I would like to create a system of categories, or my personal knowledge base, for all my bookmarks, And for all manner of other stuff that I happen to keep in my personal files.

The pattern or the system of categories should closely follow the category system in Wikipedia. See

* https://en.wikipedia.org/wiki/Wikipedia:Contents/Categories

There are 13 major categories, numerous subcategories, and sub-subcategories. The top 13 are:

* Reference
* Culture
* Geography
* Health
* History
* Human activities
* Mathematics
* Nature
* People
* Philosophy
* Religion
* Society
* Technology

The result should be a markdown file with the 13 major categories as header 2s, subcategories as header 3s, and so on.

Under every header there should be a README.md file that list the categories That should appear under this header as well as the major aspects of the categories.

Run this step-by-step.

## Prompt

You are an expert knowledge management consultant and technical writer specializing in creating comprehensive organizational systems for personal digital libraries.

## Task Overview
Create a complete personal knowledge base category system based on Wikipedia's category structure for organizing bookmarks and personal files. The system will be implemented as a hierarchical markdown structure suitable for GitHub Pages.

## Specific Requirements

### Structure Requirements
* Follow Wikipedia's 13 main categories exactly: Reference, Culture, Geography, Health, History, Human activities, Mathematics, Nature, People, Philosophy, Religion, Society, Technology
* Use Header 2 (##) for the 13 main categories
* Use Header 3 (###) for subcategories
* Use Header 4 (####) for sub-subcategories
* Maximum depth: 3 levels (Headers 2, 3, and 4 only)
* Headers 2, 3, and 4 should match Wikipedia's structure closely
* All formatting must be GitHub Flavored Markdown compatible

### Content Requirements for Each Category

1. **Category Description**: Brief explanation of what belongs in this category
2. **Subcategory Listings**: Complete list of relevant subcategories
3. **Content Examples**: Specific examples of bookmarks/files that would belong here
4. **Tagging Conventions**: Suggested tags for easy searching and filtering
5. **Organizational Tips**: Best practices for maintaining this category

### Reference Sources
* Base structure on: https://en.wikipedia.org/wiki/Wikipedia:Contents/Categories
* Maintain consistency with Wikipedia's categorization logic
* Adapt examples and tips for personal bookmark/file management

### Target Use Case
* Personal knowledge base for mixed content (research articles, tutorials, tools, entertainment, etc.)
* GitHub Pages compatible
* Suitable for bookmark management and personal file organization

## Output Format
Deliver a complete markdown file with:

1. **Main Structure**:
   * Title: "# Personal Knowledge Base Categories"
   * Introduction paragraph explaining the system
   * Table of contents linking to all 13 main categories

2. **For Each Main Category**:

   ```markdown
   ## [Category Name]

   **Description**: [What this category encompasses]

   **Subcategories**:
   ### [Subcategory 1]
   [Description and examples]

   #### [Sub-subcategory if applicable]
   [Description and examples]

   **Content Examples**:
   - [Specific examples of bookmarks/files]
   - [More examples]

   **Tagging Conventions**:
   - Primary tags: [list]
   - Secondary tags: [list]

   **Organizational Tips**:
   - [Practical advice for maintaining this category]
   - [Best practices for this type of content]
   ```

3. **README.md Content**: Include comprehensive guidance for each section

## Style Guidelines
* Use clear, concise language
* Provide actionable, practical advice
* Include diverse examples covering different content types
* Maintain professional tone suitable for personal productivity
* Ensure all markdown syntax is GitHub Pages compatible

## Constraints
* Stick closely to Wikipedia's hierarchical structure for headers 2-4
* Keep maximum depth at 3 levels
* Focus on practical usability over theoretical completeness
* Ensure examples are relevant to modern digital content management

## Expected Deliverable
A single, comprehensive markdown file ready for immediate use in a personal knowledge management system, complete with all 13 main categories, their subcategories (up to 3 levels deep), and detailed guidance for each section.
