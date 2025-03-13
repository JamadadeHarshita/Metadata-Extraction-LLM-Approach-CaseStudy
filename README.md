# A Large Language Model approach to extract Biographical Transcripts 

## **Project Overview**  
The **German Memory** archive, housed at **FernUniversität in Hagen**, preserves **subjective memories** such as **biographical interviews, autobiographies, diaries, and letters** related to sociopolitical events in Germany and beyond. This project integrates AI-driven metadata extraction to enhance accessibility and analysis of these historical materials.<br><br><br><br>




## **Data Overview**  
The raw data consists of **interview transcripts** stored in a **tab-separated values (TSV)** format, which are processed and structured to include various categories of metadata.  

### **A. Input Data Description**  
The dataset consists of interview transcripts stored in **TSV files with 12 columns**, including **spoken text, timestamps, speaker information, translations, and annotations**.  

#### **Key Columns in Input Data**  

| Column                 | Description  |
|------------------------|-------------|
| **Band**              | Identifies the segment number within the interview.  |
| **Timecode**          | The timestamp of the spoken text in HH:MM:SS format.  |
| **Sprecher**          | The speaker identifier (e.g., interviewer, participant).  |
| **Transkript**        | The original interview text.  |
| **Übersetzung**       | Translation of the spoken text.  |
| **Zwischenüberschrift** | Subheading within a section.  |
| **Hauptüberschrift (Übersetzung)** | Translated version of the main heading.  |
| **Zwischenüberschrift (Übersetzung)** | Translated version of the subheading.  |
| **Registerverknüpfungen** | Index references or linked records.  |
| **Anmerkungen**       | Additional notes on the conversation.  |
| **Anmerkungen (Übersetzung)** | Translated version of the notes.  |

### **B. The Metadata Schema**  
After processing, the extracted metadata is transformed into a structured format with **136 columns** categorized into **13 major groups** to ensure comprehensive information retrieval.  

#### **Metadata Schema Categories**  

| Category                         | Description  |
|----------------------------------|-------------|
| **General Information**          | Location, Archive ID, Document Type, Time of Creation |
| **Personal Information**         | Name, Birth Year, Gender, Pseudonym |
| **Interview Details**            | Interview Title, Duration, Interviewer, Segmentation |
| **Contact Information**          | Street, Postal Code, Phone Number |
| **Social and Demographic Info**  | Group Affiliation, Occupation, Family Status |
| **Media and Storage Details**    | Photos, Documents, Storage Medium |
| **Education and Career**         | Schooling, Career Changes, Employment Status |
| **Family and Relationship Data** | Marital History, Children’s Birth Years |
| **Political and Social Engagement** | Political Orientation, Memberships |
| **Historical and Wartime Involvement** | Military Service, Nazi-related Organizations |
| **Parental and Partner Information** | Parents’ Background, Partner’s Engagement |<br><br><br><br>







## **System Architecture**  
The system architecture is designed to efficiently process and extract structured metadata from interview transcripts while preserving context.<br><br>

![System Architecture]("C:/Users/dhans/Downloads/image (1).png")


### **Workflow**  
1. **Preprocessing**:  
   - Raw interview transcripts are cleaned to remove irrelevant data.  
   - Formatting is standardized, retaining essential fields like timestamps, speaker information, and transcribed text.  

2. **Text Segmentation**:  
   - Given the length of transcripts, the text is **segmented into smaller, context-aware chunks**.  
   - A **cosine similarity-based chunking mechanism** identifies logical breakpoints by measuring semantic similarities between adjacent sentences.  
   - This ensures that topic coherence is maintained.  

3. **Metadata Extraction with LLM**:  
   - Each segmented chunk is processed individually by **Llama-3.3 70B**, which extracts relevant metadata fields based on a predefined schema.  
   - Extracted metadata from previous chunks is passed to subsequent ones to maintain **context consistency** across segments.  

4. **Output Formatting**:  
   - The extracted metadata is **refined and structured into a standardized format (CSV, JSON)**.  
   - This ensures **ease of storage and retrieval** for researchers.<br><br><br><br>




  



## **Methodology**  
This study extracts important details from German interview transcripts using LLMs, following several key steps for accurate, structured metadata extraction.

## A. Preprocessing
- **Key Fields**: The main fields (Timecode, Sprecher, Transkript) are prioritized for extraction:
  - **Timecode**: Aligns timestamps.
  - **Sprecher**: Differentiates between interviewer and interviewee.
  - **Transkript**: Provides the raw textual content.
- **Metadata Schema**: The metadata schema was initially in TSV format, which was converted to JSON for structured data representation and error minimization.

## B. Choosing the Right Language Model
- **LLM Exploration**: Evaluated various models, focusing on German language support.
  - Models tested: LLaMA 8B, Mistral 7B, LLaMA 70B, Mixtral 8x7B (smaller and larger variants).
- **Evaluation**: Models were tested on extracting 12 key data points from a sample transcript. Results are shown in Table IV.

## C. Chunking Strategy: Context-Aware Segmentation for LLM Processing
- **Challenge**: Conventional chunking techniques led to inconsistencies.
- **Approach**: Used TF-IDF vectorization and cosine similarity to identify natural segment boundaries based on content shifts.
  - Segmentation dynamically adapts to thematic shifts in the conversation, ensuring topic coherence.
  - Visualized segmentation process shown in Figure 2.

## D. Chunk-wise Metadata Extraction
- **JSON-based Approach**: Used JSON schema to ensure structured output and map extracted details to predefined fields, improving accuracy and consistency.

## E. Post Processing
- **Final Steps**: The extracted metadata was compiled into a DataFrame and exported as a CSV file for structured storage and further analysis.<br><br><br><br>








## **Discussion**

The process of extracting structured metadata from German interview transcripts using Large Language Models (LLMs) has provided valuable insights and identified several areas for improvement.

## A. Key Findings
- **Improved Data Quality and Schema Representation**: Converting metadata to JSON ensured consistent and reliable parsing by the LLM, minimizing errors and improving data quality by focusing on key fields.
- **Effective Chunking Strategy for Context Preservation**: TF-IDF vectorization and cosine similarity allowed for natural segmentation of transcripts, maintaining contextual integrity and improving metadata extraction accuracy.
- **Performance Evaluation of Language Models**: Larger models, such as LLaMA 70B and Mixtral 8x7B, performed best, with accuracy rates of 95.83%. LLaMA 70B was chosen as the final model due to its stability and reliability.
- **Structured Prompting and JSON-Based Metadata Extraction**: Switching to a JSON-based extraction method standardized outputs, reducing inconsistencies and improving extraction continuity.

## B. Challenges and Limitations
- **Handling Large Transcripts Without Losing Context**: Chunking strategies can miss complex relationships or references across distant segments, especially when interviewees refer back to earlier points.
- **Predefined Metadata Schema Restricts Flexibility**: Modifying the schema for evolving needs or new metadata categories requires changes to the entire pipeline, limiting adaptability.
- **Variability in LLM Responses**: Different LLMs provided varying results, especially Mixtral 8x7B across platforms. This highlights the need for consistency in LLM outputs.
- **Potential Bias in Metadata Extraction**: Biases in LLMs’ training data may lead to inaccurate metadata extraction, particularly for sensitive topics. Post-processing checks are necessary to ensure neutrality.

## C. Future Improvements
- **Incorporation of Human Annotation for Quality Control**: Introducing human validation to evaluate the model’s precision and recall will refine prompts and processing logic.
- **Adaptive Chunking Strategy**: Integrating Named Entity Recognition (NER) with similarity scores could improve chunking by preserving important entities across segments.
- **Multi-Model Ensemble Approach**: Using an ensemble of models for metadata extraction, followed by aggregation and statistical validation, could improve accuracy.
- **Improved Post-Processing**: Further refining data cleaning and validation processes could improve the quality of extracted metadata.

These improvements aim to enhance the accuracy, adaptability, and efficiency of the metadata extraction pipeline.<br><br><br><br>
