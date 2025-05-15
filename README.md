# Trademark Analysis Application

A sophisticated trademark analysis system that combines machine learning models and LLM-based analysis to evaluate trademark similarities and potential conflicts.

## Table of Contents
- [Installation](#installation)
- [Requirements](#requirements)
- [Application Overview](#application-overview)
- [Core Components](#core-components)
- [Workflow](#workflow)
- [Usage](#usage)

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd trademark-application
```

2. Create and activate a virtual environment:
```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# Linux/Mac
python -m venv .venv
source .venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

## Requirements

The application requires Python 3.8+ and the following key dependencies:
- `streamlit==1.31.1`: Web interface
- `pandas==2.2.0`: Data manipulation
- `PyMuPDF==1.23.8`: PDF processing
- `pydantic==2.6.1`: Data validation
- `python-docx==1.0.1`: Word document generation
- `sentence-transformers`: Semantic similarity analysis
- `phonetics==1.0.5`: Phonetic analysis
- `openai==1.12.0`: LLM integration
- Additional NLP and ML libraries for text processing and analysis

## Application Overview

This application implements a sophisticated trademark analysis system that combines machine learning models with LLM-based analysis to evaluate trademark similarities and potential conflicts. The system processes trademark documents, extracts relevant information, and performs comprehensive similarity analysis.

### Core Components

#### 1. Data Models
- `TrademarkDetails` (BaseModel): Defines the structure for trademark information including:
  - Trademark name
  - Status
  - Serial number
  - International class numbers
  - Owner information
  - Goods/services description
  - Registration details

#### 2. Document Processing
Key functions:
- `read_pdf()`: Extracts text from PDF documents
- `split_text()`: Divides text into manageable chunks
- `extract_trademark_details_code1()` and `extract_trademark_details_code2()`: Extract trademark information from different document formats

#### 3. Similarity Analysis
The system implements a multi-stage analysis pipeline:

##### ML-Based Analysis
- `ml_semantic_match()`: Evaluates semantic similarity between trademarks
- `ml_phonetic_match()`: Assesses phonetic similarity
- Threshold-based filtering:
  - Marks below 0.75 similarity are rejected
  - Marks above 0.85 similarity are automatically accepted
  - Marks between 0.75-0.85 are sent for LLM analysis

##### LLM Analysis
- `analyze_borderline_match()`: Performs detailed analysis of borderline cases
- Uses GPT models to evaluate:
  - Semantic relationships
  - Phonetic similarities
  - Market context
  - Consumer confusion potential

#### 4. Conflict Assessment
- `compare_trademarks()`: Primary comparison function
- `assess_conflict()`: Detailed conflict analysis
- `validate_trademark_relevance()`: Validates similarity findings

#### 5. Opinion Generation
- `generate_trademark_opinion()`: Creates comprehensive analysis reports
- `export_trademark_opinion_to_word()`: Exports results to Word documents
- Includes sections for:
  - Section One Analysis
  - Section Two Analysis
  - Section Three Analysis
  - Web Common Law Analysis

## Workflow

The application implements a sophisticated multi-stage workflow for trademark analysis, combining machine learning models, LLM-based analysis, and comprehensive reporting. Below is a detailed breakdown of each stage:

### 1. Document Processing and Information Extraction

#### Initial Document Handling
- **PDF Processing**: The system begins by processing uploaded PDF documents using `read_pdf()`, which extracts text while optionally excluding headers and footers
- **Text Normalization**: Extracted text undergoes preprocessing through `preprocess_text()` to standardize formatting and remove irrelevant elements
- **Chunk Management**: Large documents are split into manageable chunks using `split_text()` with a default token limit of 1500

#### Information Extraction
- **Trademark Details Parsing**: The system employs two primary extraction methods:
  - `extract_trademark_details_code1()`: Handles standard format documents
  - `extract_trademark_details_code2()`: Processes alternative document formats
- **Key Information Extraction**:
  - Serial numbers via `extract_serial_number()`
  - Ownership details through `extract_ownership()`
  - Registration numbers using `extract_registration_number()`
  - International class numbers and goods/services via `extract_international_class_numbers_and_goods_services()`
  - Design phrases through `extract_design_phrase()`

### 2. Initial ML-Based Analysis

#### Semantic Similarity Analysis
- **Primary Analysis**: `ml_semantic_match()` evaluates semantic relationships between trademarks using sentence transformers
- **Threshold Implementation**:
  - High confidence matches (>0.85): Automatically accepted
  - Low confidence matches (<0.75): Automatically rejected
  - Borderline cases (0.75-0.85): Queued for LLM analysis

#### Phonetic Analysis
- **Sound-Based Comparison**: `ml_phonetic_match()` performs phonetic similarity analysis
- **Multiple Methods**:
  - Metaphone-based comparison
  - Levenshtein distance calculation
  - First-word phonetic equivalence check

### 3. LLM-Based Analysis for Borderline Cases

#### Threshold-Based Processing
- **Initial ML Model Processing**:
  - All trademarks are first processed through ML models for both phonetic and semantic similarity checks
  - Base threshold set at 0.8 with a ±0.05 margin
  - Processing Categories:
    - **Automatic Rejection** (< 0.75): Marks with similarity scores below 0.75 are immediately rejected without LLM analysis
    - **Automatic Acceptance** (> 0.85): Marks with similarity scores above 0.85 are automatically accepted
    - **LLM Analysis Queue** (0.75 - 0.85): Only marks within this range are sent for detailed LLM analysis

#### LLM Analysis Pipeline
- **Borderline Case Processing**: `analyze_borderline_match()` handles cases within the 0.75-0.85 similarity range
- **Comprehensive Analysis**:
  - Semantic relationship evaluation
  - Phonetic similarity assessment
  - Market context consideration
  - Consumer confusion potential analysis
  - Goods/services overlap evaluation

#### Result Compilation
- **Final Results Table**:
  - Includes all marks with similarity scores > 0.85 (automatically accepted)
  - Includes marks that passed LLM analysis (0.75-0.85 range)
  - Excludes marks with similarity scores < 0.75 (automatically rejected)
- **Result Categories**:
  - Phonetic matches
  - Semantic matches
  - Combined similarity scores
  - LLM analysis outcomes for borderline cases

#### Decision Making
- **Contextual Analysis**: LLM evaluates multiple factors:
  - Industry-specific considerations
  - Market segment overlap
  - Consumer perception factors
  - Historical trademark usage patterns
- **Final Classification**:
  - High confidence matches (> 0.85)
  - LLM-validated matches (0.75-0.85)
  - Rejected matches (< 0.75)

### 4. Conflict Assessment and Validation

#### Primary Conflict Analysis
- **Trademark Comparison**: `compare_trademarks()` performs comprehensive comparison
- **Multi-factor Evaluation**:
  - Name similarity assessment
  - Class number overlap
  - Goods/services relationship
  - Market segment analysis

#### Validation and Refinement
- **Relevance Validation**: `validate_trademark_relevance()` confirms similarity findings
- **Conflict Assessment**: `assess_conflict()` provides detailed conflict analysis
- **Crowded Field Analysis**: `analyze_crowded_field()` evaluates market saturation

### 5. Report Generation and Export

#### Opinion Generation
- **Comprehensive Analysis**: `generate_trademark_opinion()` creates detailed reports
- **Section-wise Analysis**:
  - Section One: Primary similarity analysis
  - Section Two: Detailed conflict assessment
  - Section Three: Market impact analysis
  - Web Common Law: Online presence evaluation

#### Document Export
- **Word Document Creation**: `export_trademark_opinion_to_word()` generates formatted reports
- **Content Processing**: `process_opinion_content()` structures the analysis
- **Formatting**: 
  - Cell text formatting via `format_cell_text()`
  - Paragraph formatting through `format_paragraph_text()`

### 6. Additional Analysis Features

#### Web Common Law Analysis
- **Online Presence**: `web_law_page()` processes web-based trademark information
- **Image Processing**: 
  - Page conversion via `convert_pages_to_pil_images()`
  - Image encoding through `encode_image()`
  - Single image processing using `process_single_image()`

#### Component Analysis
- **Consistency Checking**: `component_consistency_check()` evaluates trademark components
- **Crowded Field Analysis**: `analyze_crowded_field()` assesses market saturation
- **Section-wise Analysis**:
  - Section Four: Web presence analysis
  - Section Five: Component consistency
  - Section Six: Comprehensive evaluation

## Usage

1. Start the application:
```bash
streamlit run app_main.py
```

2. Upload trademark documents through the web interface

3. View analysis results and generated reports

## Additional Features

- Crowded field analysis (`analyze_crowded_field()`)
- Component consistency checking (`component_consistency_check()`)
- Web common law analysis
- Comprehensive opinion generation with multiple analysis sections

## Notes

- The system uses a combination of ML models and LLM analysis for optimal results
- Thresholds can be adjusted based on specific requirements
- The application supports multiple document formats and analysis methods 