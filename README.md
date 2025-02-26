# AIWebinar
Follow Along AI Webinar of Feb 26, 2025

## Vectors:
Create a table for our Vector DB:

    CREATE TABLE vectortable (txt VARCHAR(1000), vec VECTOR(FLOAT, 2))


Insert vector [1,0] 

    INSERT INTO vectortable VALUES ('our first vector', TO_VECTOR('1,0', FLOAT))

Insert vector [0,1]

    INSERT INTO vectortable VALUES ('our second vector', TO_VECTOR('0,1', FLOAT))

Use Dot Product for Vector Search

    SELECT TOP 2 vectortable.txt,VECTOR_DOT_PRODUCT(vec, TO_VECTOR('1,0', FLOAT)) AS similarity FROM vectortable ORDER BY similarity DESC

## Embedding From Code (SQL Procedure):

    SELECT AIWebinar.EmbeddingDemo_GetEncoding('Our First Embedding')

## Vector Search with Embedding Data Type:

Declare Embedding:

    INSERT INTO %Embedding.Config (Name, Configuration, EmbeddingClass, VectorLength, Description)
      VALUES ('my-openai-config', 
              '{"apiKey":"<api key>", 
                "sslConfig": "llm_ssl", 
                "modelName": "text-embedding-3-small"}',
              '%Embedding.OpenAI', 
              1536,  
              'a small embedding model provided by OpenAI') 

Embedding from Embedding Data Type
    
    SELECT EMBEDDING('Our second embedding.','my-openai-config')

Create Table for Embedding Demo

    CREATE TABLE Embedding.CarExample (CarReview VARCHAR(200),MagazineName VARCHAR(30),CarReviewEmbedding EMBEDDING('my-      openai-config','CarReview'), NameEmbedding EMBEDDING('my-openai-config','MagazineName') )

Insert Toyota Review

    INSERT INTO Embedding.CarExample (CarReview, MagazineName) VALUES ('Toyota is a world class car company.', 'Fake Car Magazine 1')

Insert Honda Review

    INSERT INTO Embedding.CarExample (CarReview, MagazineName) VALUES ('The Honda is extremely reliable.', 'Fake Car Magazine 2')

Insert Yugo Review

![images](https://github.com/user-attachments/assets/8710f94a-0a86-45ce-b02d-295756294c2a)

    INSERT INTO Embedding.CarExample (CarReview, MagazineName) VALUES ('The Yugo, though cheap, may have been one of the worst cars ever made.', 'Fake Car Magazine 3')

Use a Vector Search with Embedding Data Type

    SELECT TOP 2 CarReview FROM Embedding.CarExample ORDER BY VECTOR_DOT_PRODUCT(CarReviewEmbedding, EMBEDDING(?)) DESC

### RAG
[RAG Demo](https://github.com/Ari-Glikman/AIWebinar/blob/main/Jupyter%20Notebook/langchain-rag.ipynb)

### Hybrid Search
[Pre-Requirement for Hybrid Search](https://github.com/Ari-Glikman/AIWebinar/blob/main/Jupyter%20Notebook/sql_demo.ipynb)

[Hybrid Search](https://github.com/Ari-Glikman/AIWebinar/blob/main/Jupyter%20Notebook/sql_demo.ipynb)
