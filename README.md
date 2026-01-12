Retrieval Augmented Generation for Data Overloading


🚀 Overcome Information Overload with Smarter Retrieval!
In today's data-rich environment, finding the right information at the right time is a challenge. Traditional search methods often return too many irrelevant results, leading to 'data overloading' and inefficient decision-making. This project presents a robust solution: a Retrieval Augmented Generation (RAG) system designed to help you efficiently query vast amounts of unstructured data and get precise, contextually relevant answers.

✨ What is it?
This system combines the power of advanced language models with a sophisticated document retrieval mechanism. Instead of relying solely on a large language model's (LLM) general knowledge, our RAG system first retrieves highly relevant snippets from your private or domain-specific documents and then augments the LLM's response generation with this specific context. The result? More accurate, grounded, and useful answers, directly from your data.

💡 How Does It Solve Data Overloading?
Smart Ingestion: Your documents (text, PDFs, JSON, CSV, etc.) are processed, chunked, and transformed into numerical representations (embeddings).
Efficient Retrieval: When you ask a question, the system intelligently searches through your embedded document store to find the most relevant pieces of information.
Contextual Generation: These retrieved snippets act as a focused 'knowledge base' for the LLM, enabling it to generate an answer that is precise and directly supported by your data, cutting through the noise.

🛠️ Key Technologies & Features
LangChain: Orchestrates the entire RAG pipeline, from document processing to LLM interaction.
ChromaDB: A lightweight, fast, and scalable vector database for storing and retrieving document embeddings.
HuggingFace Embeddings (all-MiniLM-L6-v2): Generates high-quality vector representations of text for efficient similarity search.
Ollama (llama3-chatqa): Leverages the powerful llama3-chatqa model for natural language understanding and generation, providing context-aware answers.
MCP (Model Context Protocol): Enables modular and scalable server operations.
🚀 Getting Started
1. Installation
First, install the necessary Python packages:

pip install langchain langchain_community chromadb sentence-transformers ollama mcp asyncio-mqtt python-dotenv
2. Run Ollama Server
You need to have an Ollama server running and the llama3-chatqa model downloaded. If you don't have Ollama installed, follow the instructions on Ollama's official website.

For Linux/Mac (in a terminal):

curl -fsSL https://ollama.ai/install.sh | sh
ollama pull llama3
ollama run llama3-chatqa # Start the model server
Note for Colab Users: If you are running this in Google Colab, the http://localhost:11434 URL for Ollama will not work directly. You will need to set up Ollama to be accessible externally (e.g., using ngrok or similar tools) and update the ollama_url in the CustomOllamaLLM constructor accordingly.

3. Initialize & Ingest Documents
Run the provided Python code to initialize the RAGSystem and ingest your documents.

# ... (Your RAGSystem, CustomOllamaLLM, and main() function code)

if __name__ == "__main__":
    # Example usage
    rag = RAGSystem(persist_directory="./my_vector_db")
    rag.initialize()

    # Create sample documents for testing (if you don't have your own)
    # create_sample_documents() # Call if you have this helper function

    # Ingest your actual documents
    documents_to_ingest = [
        {"file_path": "/path/to/your/document1.txt"},
        {"file_path": "/path/to/your/report.pdf"},
        # Add more documents here
    ]

    for doc_info in documents_to_ingest:
        print(f"Ingesting {doc_info['file_path']}...")
        result = rag.ingest_document(doc_info["file_path"])
        print(f"  Result: {result}")

    print(f"\nSystem Stats: {rag.get_stats()}")
4. Query the System
Once documents are ingested, you can start asking questions:

# ... (continued from above example)

    print("\n" + "="*50)
    print("Interactive RAG Querying")
    print("="*50)

    while True:
        question = input("\nYour question: ")
        if question.lower() in ["quit", "exit"]:
            break

        print("Processing your question...")
        result = rag.query(question, k=3) # Retrieve top 3 relevant documents

        print(f"Answer: {result.get('answer', 'No answer found.')}")
        sources = result.get('sources', [])
        if sources:
            print("--- Sources Used ---")
            for i, source in enumerate(sources):
                print(f"  Source {i+1}: {source.get('file_name', 'Unknown File')} (Similarity: {source.get('similarity', 'N/A'):.4f})")
                print(f"    Content Snippet: {source.get('content', 'N/A')}")
                print("  " + "-"*10)
            print("--------------------")
🔮 Future Enhancements
Support for more document types: Expand DocumentProcessor to handle more file formats (e.g., DOCX, HTML).
Advanced Chunking Strategies: Implement more sophisticated chunking logic for better context preservation.
Asynchronous API Calls: Optimize CustomOllamaLLM for fully asynchronous operations.
User Interface: Develop a simple web UI for easier interaction and visualization.
Fine-tuning Embeddings/LLM: Explore domain-specific fine-tuning for even higher accuracy.
