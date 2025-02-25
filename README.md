# AIWebinar
Examples To Follow Along AI Webinar of Feb 26, 2025

## Vectors:
    ```Shell
    CREATE TABLE vectortable (txt VARCHAR(1000), vec VECTOR(FLOAT, 2))
    ```

INSERT INTO vectortable VALUES ('our first vector', TO_VECTOR('1,0', FLOAT))

INSERT INTO vectortable VALUES ('our second vector', TO_VECTOR('0,1', FLOAT))

SELECT TOP 2 vectortable.txt,VECTOR_DOT_PRODUCT(vec, TO_VECTOR('1,0', FLOAT)) AS similarity FROM vectortable ORDER BY similarity DESC

## Embeddings:

SELECT AIWebinar.EmbeddingDemo_GetEncoding('Our First Embedding')

## Vector Search:

INSERT INTO %Embedding.Config (Name, Configuration, EmbeddingClass, VectorLength, Description)
  VALUES ('my-openai-config', 
          '{"apiKey":"<api key>", 
            "sslConfig": "llm_ssl", 
            "modelName": "text-embedding-3-small"}',
          '%Embedding.OpenAI', 
          1536,  
          'a small embedding model provided by OpenAI') 

SELECT EMBEDDING('Our second embedding.','my-openai-config')

CREATE TABLE Embedding.CarExample (CarReview VARCHAR(200),MagazineName VARCHAR(30),CarReviewEmbedding EMBEDDING('my-openai-config','CarReview'), NameEmbedding EMBEDDING('my-openai-config','MagazineName') )

INSERT INTO Embedding.CarExample (CarReview, MagazineName)
VALUES ('Toyota is a world class car company.', 'Fake Car Magazine 1')

INSERT INTO Embedding.CarExample (CarReview, MagazineName)
VALUES ('The Honda is extremely reliable.', 'Fake Car Magazine 2')

INSERT INTO Embedding.CarExample (CarReview, MagazineName)
VALUES ('The Yugo, though cheap, may have been one of the worst cars ever made.', 'Fake Car Magazine 3')

SELECT TOP 2 CarReview FROM Embedding.CarExample ORDER BY VECTOR_DOT_PRODUCT(CarReviewEmbedding, EMBEDDING(?)) DESC
