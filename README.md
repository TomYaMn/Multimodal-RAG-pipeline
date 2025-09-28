# Multimodal-RAG-pipeline CHATBOT
A chatbot that answers user queries based on information extracted from a PDF containing both text and images.


## Section 1
This Section outlines the architecture of a multimodal Retrieval-Augmented Generation (RAG) system designed to answer queries by leveraging both text and image data from documents.

The core of this system is a multi-vector retriever that processes documents in two ways: by extracting raw data and by creating condensed summaries. This dual approach allows for more effective and contextually relevant information retrieval.
<img width="1796" height="646" alt="image_1" src="https://github.com/user-attachments/assets/a3fb13f9-7654-4539-8c76-8252bd3e47c7" />

### 1.1. Raw Data Extraction
separate and store the distinct elements (text and images) from each document.
Process: The unstructured library is used to parse the documents.
  - Text Chunks: The textual content is extracted and split into manageable chunks.
  - Images: All images are extracted and saved.

### 1.2. Data Summarization
Utilizing OpenAI platform api key create concise summaries of the extracted text and images, providing a high-level context that can be used for more efficient retrieval.
Process:
  - Text Summaries: The extracted text chunks are passed to a language model to generate brief summaries.
  - Image Summaries: The extracted images are passed to a multi-modal model to generate descriptive captions or summaries.

### 1.3. Data Storage and Linking
Store both the raw data and the summaries in a way that they can be linked and retrieved together.
Process:
  - A unique ID is generated for each source document.
  - Both the raw chunks (text and images) and their corresponding summaries are stored in a vector database.
Crucially, all these related pieces of information (raw text, raw image, text summary, image summary) are linked together using the same unique ID.

### 1.4. Multi-Vector Retrieval and Generation
Fnd the most relevant text and image data related to handles an incoming user query and generates an answer..
Process:
  - The user's query is used to search the vector database.
  - The system searches against both the raw data and the summaries. This allows it to find matches based on either specific details (from the raw data) or broader context (from the summaries).
  - When a relevant piece of information is found (either a summary or a raw chunk), the system uses the unique ID to retrieve all the other linked documents (both raw and summarized).

### 1.5. Generative Augmentation
The retrieved text chunks, images, and their summaries are all passed to a powerful multi-modal language model to generate a comprehensive and accurate answer.

## Section 2
This section illustrates how the extracted and summarized data is utilized in a production environment. The entire system is containerized using Docker to store and preserve the data and its linkages, ensuring a consistent and scalable deployment.
<img width="1491" height="867" alt="image_2" src="https://github.com/user-attachments/assets/ea60f302-0821-4805-a092-f5227bc9b0d3" />


### 2.1. Data Persistence and Linkage
Objective: To maintain the integrity of the raw data, summaries, and their unique IDs for production use.
  - The vector database, containing all processed content and its relational IDs, is run as a persistent service within a Docker container. Docker volumes are used to ensure that all data and linkages are preserved across deployments and container restarts.

### 2.2. Production Retrieval System
Objective: To deploy the retrieval and generation components as a scalable service.
  - The multi-vector retriever and the generative language model are packaged into a Docker container. This containerized application is deployed to our production environment, where it connects to the persistent vector database. When a user query is received, this service retrieves the relevant linked data (both raw and summarized) to generate a final, context-aware answer.

