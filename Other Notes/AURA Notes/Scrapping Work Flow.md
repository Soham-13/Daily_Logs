
## 1. Web Scraping

- Crawl the target website and collect text content.
- Add any discovered links to the crawl queue.
- Save crawled URLs in an Excel file.
- Output: Raw PDF (text only) + Excel of links.

## 2. PDF to Markdown

- Convert the PDF into a Markdown (.md) file.
- Markdown is lightweight text and easier for LLMs to process.
- Output: Clean .md file.

## 3. File Hashing

- Generate a unique Hash ID (UUID/GUID) for each PDF.
- Store the mapping `{ fileName, hashId }` in a `hash.json` file.
- This keeps consistent references across the pipeline.

## 4. Chunking

- Split the .md file into chunks (e.g., 500 words).    
- Each chunk is stored as JSON with:    
    - `hashId`
    - `pageNumber`
    - `chunkText`
- Output: Multiple JSON files per PDF.

## 5. Summary Generation

- Use Google Gemini to generate a summary for each chunk.
- Input for summary = previous, current, and next chunk (for context).
- Store summary in the same JSON file under a new `summary` field.

Example:

```json
{
  "hashId": "abc123",
  "pageNumber": 4,
  "chunkText": "....",
  "summary": "Short summary of this chunk"
}
```

## 6. Summary Embedding

- Generate embeddings for each summary.
- Format them for Neon + Pinecone vector database.
- Store the vectors in Pinecone for semantic search.

## Final Flow Recap

1. Scraping → PDF + Excel
2. PDF → Markdown
3. Hashing → hash.json
4. Markdown → JSON chunks
5. Chunks → Summaries with Gemini
6. Summaries → Embeddings → Pinecone




