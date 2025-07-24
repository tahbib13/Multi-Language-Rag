# Multi-Language-Rag

# At first update the pdf file, because I want to adjust the Bangla Font new file is : Updated_HSC26-Bangla1st-Paper.pdf

#Below File is the requirements of this Task ->

Work start with : !pip install -r requirements.txt

#WorkFlow of my task ->

RAG system workflow is well-structured and reflects a solid understanding of how retrieval-augmented generation works. Here's a clear restatement of your pipeline:

Document Load – Ingest PDF content using a tool like PyPDFLoader.

Text Split (Subsection) – Break down the content into manageable, semantically meaningful chunks using RecursiveCharacterTextSplitter.

Embedding Model – Convert each chunk into dense vector embeddings using a multilingual model like paraphrase-multilingual-mpnet-base-v2.

Vector Storage – Store embeddings in a vector database (e.g., FAISS). User queries are also embedded and compared to find the most relevant chunks.

Query + Retrieval + Prompting – Combine the user query with the top-k relevant chunks, pass it through a QA pipeline (sagorsarker/mbert-bengali-tydiqa-qa), and generate a response.

Question/Answer section :
________________________________________

🔹 1. What method or library did you use to extract the text, and why?
Library Used:
langchain_community.document_loaders.PyPDFLoader was used to extract text from the PDF file.
Why this method?
This loader is a convenient wrapper around pdfplumber, which:
•	Preserves text layout better than standard extractors.
•	Handles multiple pages as individual documents (doc.page_content).
•	Allows easier integration with Langchain’s ecosystem for downstream tasks (splitting, embedding, indexing).
Formatting Challenges Faced?
Yes — Bangla PDFs often:
•	Use non-Unicode fonts (SutonnyMJ, etc.), leading to broken or garbled output.
•	Contain page artifacts, header/footer noise, or line breaks mid-sentence.
•	Have alignment issues, especially when converted from scanned images.
If the text looks disconnected or misaligned, it’s likely due to these encoding or layout problems.
________________________________________
🔹 2. What chunking strategy did you choose? Why is it good for semantic retrieval?
Method Used:
RecursiveCharacterTextSplitter with:
•	chunk_size = 500
•	chunk_overlap = 100
Why this works well?
•	Character-based chunking ensures consistent size and avoids over-reliance on paragraph or sentence boundaries (which may be broken in poorly formatted PDFs).
•	Recursive approach first tries to split by larger units (e.g., paragraph, sentence), and if not possible, falls back to characters.
•	Overlap = 100 keeps contextual continuity between adjacent chunks, preventing important phrases from being cut.
✅ This is effective for semantic retrieval because it balances context preservation with manageable chunk sizes for embedding models.
________________________________________
🔹 3. What embedding model did you use? Why did you choose it?
Model Used:
paraphrase-multilingual-mpnet-base-v2 from HuggingFace (via HuggingFaceEmbeddings)
Why this model?
•	It supports 100+ languages, including Bangla.
•	Optimized for semantic textual similarity, paraphrasing, and information retrieval.
•	Trained using contrastive learning to capture meaning beyond literal word overlap.
✅ It performs well for multilingual semantic similarity, making it ideal for your Bengali QA context.
________________________________________
🔹 4. How are you comparing the query with your stored chunks? Why this method and storage setup?
Vector Similarity Search with:
•	FAISS as the vector store (FAISS.from_texts)
•	Similarity metric: Likely cosine similarity (default in FAISS for dense embeddings)
Why this method/setup?
•	FAISS is fast and memory-efficient for high-dimensional nearest neighbor search.
•	Embeddings capture semantic meaning; FAISS efficiently retrieves the k most similar text chunks for a query.
•	No need to scan full documents — just semantically relevant pieces are passed to the QA model.
✅ This setup scales well and is ideal for real-time or lightweight retrieval tasks.
________________________________________
🔹 5. How do you ensure the question and document chunks are compared meaningfully?
Approach:
•	First, the embedding similarity ensures only relevant chunks are selected based on vector space proximity.
•	Then, the sagorsarker/mbert-bengali-tydiqa-qa QA pipeline receives both the question and the concatenated context.
If the query is vague or missing context?
•	FAISS may return irrelevant chunks (since similarity is based on surface-level meaning).
•	QA pipeline may produce hallucinated or default answers, especially if no chunk is clearly answerable.
✅ To improve:
•	Add query reformulation, or use query rewriting models for vague input.
•	Use reranking models like BGE-Reranker for better chunk ranking.
________________________________________

🔹 6. Do the results seem relevant? If not, what might improve them?
<img width="865" height="468" alt="image" src="https://github.com/user-attachments/assets/1be1eafa-e5da-416c-94d4-91ffc93d23db" />


✅ Summary Table:

<img width="965" height="462" alt="image" src="https://github.com/user-attachments/assets/05ab1d3b-9d9f-4f5c-8012-2dfc7843f55b" />




