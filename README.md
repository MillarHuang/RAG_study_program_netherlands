
# Background of project:
This is a repo that contains a workflow of building a RAG based on the study program details information in the Netherlands, including web scrapping step and RAG building step.

### Environment requirement
* This project requires **Python 3.9** or above.
* The environment containing the dependencies is listed in the **requirements.txt** file.
* The web scrapping step needs to use Selenium and Chromedriver, installing instruction can be refered to [https://medium.com/@patrick.yoho11/installing-selenium-and-chromedriver-on-windows-e02202ac2b08](https://medium.com/@patrick.yoho11/installing-selenium-and-chromedriver-on-windows-e02202ac2b08)

## Instruction:

### 1.Web Scrapping to get program details data:
    *Data_Extraction_Web_Scraping.ipynb
        -This jupyter notebook is used to 
            1. Scrape HTML content from the website that summarizes study programs in the Netherlands: [https://www.studyinnl.org/dutch-education/studies](https://www.studyinnl.org/dutch-education/studies).  The HTML content is scraped according to study programs available in different cities in the Netherlands and saved in the folder "html_scrapped"

            2. Parse the HTML content and extract program details. The extracted information is then structured into natural language text and saved in a CSV file: **Program_info_netherlands_text.csv**, where each row contains the descriptive text for one study program.

### 2.RAG building:
    *project.ipynb
        -This jupyter notebook is used to
            1.Generate the embedding vector for the descriptive text for each study program from **Program_info_netherlands_text.csv**, and save it in a CSV file: **Program_info_netherlands_embeddings.csv**. The embedding model used is **text-embedding-3-large** from openAI

            2.Get custom query completion:
                2.1.Create the custom text prompt:
                    * 1.
                    Given a question, calculate the distance between the question text and all the text in database, then sort the dataframe text (database) by the distance from least to most.
                    * 2.
                    Create the context in the custom text prompt: retrieve the most relevant text (with shortest distances) from database, keep adding the most relevant text to the context until input prompt reach the maximum #tokens limit, where #tokens in input prompt= #tokens in prompt template + query question + context
                    * 3.
                    Combine the prompt template, query question and context to compose a custom text prompt

                * 2.2 Send the custom text prompt to the (Chat)Completion Model to query a response. The ChatCompletion model used is **gpt-4o** from openAI
                
            3. Compare the differences between the answer from a basic `Completion` model query and the answer from the custom query.

