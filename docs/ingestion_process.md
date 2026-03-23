# LightRAG Ingestion Process

LightRAG employs a sophisticated graph-based text indexing paradigm to ingest raw text data, transforming it into a structured knowledge graph that facilitates efficient and comprehensive retrieval. The ingestion process involves several key steps:

## 1. Document Chunking

The initial step involves breaking down large external text corpora into smaller, more manageable segments.
-   **Chunk Size:** Documents are typically segmented into chunks of **1200 tokens** with an **overlap of 100 tokens** between consecutive chunks. This ensures that context is preserved across chunk boundaries.

## 2. Entity and Relationship Extraction

For each processed text chunk, a Large Language Model (LLM) is utilized to identify and extract crucial information:
-   **Entities (Nodes):** The LLM identifies various entities within the text, such as names, dates, locations, organizations, and events. These become the nodes in the knowledge graph.
-   **Relationships (Edges):** The LLM also extracts the relationships or connections between these identified entities. These relationships form the edges of the graph.

## 3. LLM Profiling for Key-Value Pair Generation

After initial extraction, LightRAG leverages an LLM-empowered profiling function to generate rich, summarized representations for each entity and relationship:
-   **Keys:** For each extracted entity, its name serves as the primary index key. For relationships, the LLM may generate multiple keywords derived from the context, including global themes from connected entities, to serve as index keys.
-   **Values:** A concise text paragraph is generated, summarizing relevant snippets from the original text (or the relationship context). This summary acts as the value associated with the entity or relationship, providing descriptive information.

## 4. Deduplication

To optimize the knowledge graph and prevent redundancy, a deduplication process is applied:
-   Identical entities and relationships identified across different chunks or multiple extractions are merged. This ensures that the graph remains efficient and accurate by minimizing redundant information.

## 5. Knowledge Graph Construction

The deduplicated and profiled entities and relationships are then used to build the comprehensive knowledge graph:
-   **Nodes:** Each unique entity, along with its summarized description and associated metadata (e.g., entity type, source chunk ID, file path), becomes a node in the graph.
-   **Edges:** Each unique relationship, including its summarized description, keywords, weight, source entity, target entity, source chunk ID, and file path, forms an edge connecting the respective nodes.

## 6. Vector Database Integration (Embeddings)

LightRAG combines graph structures with vector representations for efficient retrieval:
-   **Embeddings for Nodes and Edges:** The keys (entity names, relationship keywords) and their corresponding summarized descriptions (values) are combined and then embedded into high-dimensional vectors using an embedding model (e.g., `gemini-embedding-001`).
-   **Vector Storage:** These embeddings are stored in dedicated vector databases (`entities_vdb` and `relationships_vdb`). This enables rapid and precise retrieval of relevant graph elements through semantic similarity searches when a query is made.

This graph-based ingestion approach allows LightRAG to capture complex interdependencies among entities and provides a more nuanced understanding of the text data compared to traditional flat data representations.