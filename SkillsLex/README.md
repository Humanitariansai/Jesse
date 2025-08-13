# Project Jesse: Cloud-Based Educational and Career Guidance System

## Executive Summary

Project Jesse, named after pioneering vocational guidance counselor Jesse B. Davis, represents a comprehensive cloud-based initiative designed to revolutionize educational and career guidance. This proposal details Phase I of the project, which focuses on data gathering and skills mapping from three critical sources: job listings, academic syllabi, and job titles. By integrating advanced natural language processing, discriminating terms analysis, and semantic enrichment through the SkillsLex framework, Project Jesse will create a dynamic, data-driven foundation to help students identify which degree programs and courses align with their career aspirations.

The project honors Jesse B. Davis's legacy as the first implementer of systematic school counseling by leveraging modern cloud technology to advance his vision of connecting education with career development. This proposal outlines the core data gathering components, technical implementation approach, anticipated agentic workflows, and research publications that will emerge from this initiative.

## 1. Project Objective: Skills Mapping and Educational Guidance System

### Primary Objectives

Project Jesse aims to develop a cloud-based platform that will:

1. Collect and analyze skills data from job listings, academic syllabi, and job titles
2. Create comprehensive skills maps using discriminating terms analysis and semantic enrichment
3. Establish clear connections between educational pathways and career opportunities
4. Provide students with data-driven guidance for degree and course selection based on career goals

### Solution Architecture

#### Data Integration Module:

- **Job Listings Collection Engine**: 
  - Automated scraping and ingestion of job postings from major employment platforms
  - Natural language processing for initial text normalization and segmentation
  - Cloud storage for efficient management of large-scale job listing corpus

- **Academic Syllabi Aggregation System**:
  - Structured collection of course syllabi from participating educational institutions
  - Extraction of learning outcomes, competencies, and skills from course descriptions
  - Standardization of educational content for cross-institutional comparison

- **Job Title Taxonomy Builder**:
  - Comprehensive mapping of job titles across industries and sectors
  - Hierarchical organization of positions with skill requirement overlaps
  - Temporal analysis of evolving job title requirements and emerging positions

#### Skills Mapping and Analysis Engine:

- **Discriminating Terms Identification System**:
  - Implementation of Poisson distribution-based statistical analysis to identify job-specific terminology
  - Calculation of term significance using logarithmic formulation to avoid floating-point errors
  - Cloud-based parallel processing for efficient large-scale corpus comparison

- **SkillsLex Semantic Enrichment Pipeline**:
  - Development of comprehensive lexical dictionary entries for identified skills terms
  - Enrichment with semantic relationships (synonymy, hypernymy/hyponymy, meronymy/holonymy)
  - Integration with educational competency frameworks and industry-specific terminology

- **Cross-Domain Alignment System**:
  - Mapping between job skills, educational outcomes, and professional competencies
  - Identification of skills gaps between educational offerings and industry demands
  - Analysis of transferable skills applicable across multiple domains

## 2. Data Gathering Methodology

### 2.1 Job Listings Skills Mapping

To accurately extract skills data from job listings, Project Jesse employs the Discriminating Terms approach, which statistically identifies terminology that appears significantly more frequently in job postings than in general web content.

#### Corpus Development:
- Collection of one million job descriptions from diverse sources
- Parallel collection of one million general web content articles for comparison
- Pre-processing to normalize text, remove stopwords, and apply lemmatization

#### Statistical Analysis:
- Application of Poisson distribution to determine the probability of term occurrence:

```
p(n|N, f) = e^(-Nf) · (Nf)^n / n!
```

Where:
- n is the frequency of a term in job listings
- N represents the total word count in job listings
- f indicates the term's expected frequency based on general web content

- Logarithmic reformulation to avoid computational issues:

```
ln p(n|N, f) = -Nf + n ln(Nf) - ln(n!)
```

- For large values of n, Stirling's approximation is used to compute n!

#### Semantic Enrichment:
- Integration with NLTK, spaCy, and PyDictionary for linguistic feature extraction
- Identification of synonyms, hyponyms, meronyms, and definitions for each significant term
- Custom heuristic methods for acronym detection and annotation
- Structured representation of skills terms in the SkillsLex framework

### 2.2 Academic Syllabi Skills Mapping

The academic syllabi component of Project Jesse focuses on extracting and standardizing skills and competencies from educational materials, creating a bridge between academic content and workforce needs.

#### Collection and Normalization:
- Structured ingestion of syllabi from diverse educational institutions
- Extraction of course descriptions, learning outcomes, and assessment criteria
- Standardization of academic terminology across different institutional contexts

#### Skills Identification:
- Application of Named Entity Recognition (NER) models fine-tuned for educational content
- Mapping of learning outcomes to standardized skills taxonomies
- Cross-referencing with accreditation frameworks to ensure comprehensive coverage

#### Educational Pathway Analysis:
- Tracking skill development progression through course sequences
- Identification of foundational, intermediate, and advanced skill development opportunities
- Analysis of skill coverage gaps in existing curriculum structures

### 2.3 Job Title Skills Mapping

This component creates a comprehensive mapping between job titles and their associated skill requirements, enabling clear career pathway visualization.

#### Job Title Taxonomy Development:
- Hierarchical classification of job titles across industries
- Analysis of title variations and standardization of nomenclature
- Temporal tracking of emerging job categories and evolving skill requirements

#### Role-Specific Skills Analysis:
- Extraction of core competencies associated with specific job titles
- Identification of distinguishing skills between related positions
- Analysis of career progression paths and associated skill acquisition requirements

#### Industry Alignment:
- Mapping job titles to industry-specific frameworks and standards
- Cross-sector analysis of transferable skills and competencies
- Identification of industry-specific terminology and certification requirements

## 3. SkillsLex Framework Implementation

At the core of Project Jesse is the SkillsLex framework, a comprehensive lexical dictionary for employment skills that provides rich semantic context for skills terminology. The implementation includes:

### Lexical Entry Structure:
- **Definition**: Clear explanation of the skill in employment contexts
- **Classification**: Categorization within broader skill domains
- **Synonymy**: Alternative terms and phrasing used across industries
- **Hypernymy/Hyponymy**: Hierarchical relationships (broader/narrower skills)
- **Meronymy/Holonymy**: Component skills and larger skill sets
- **Verb Relations**: Actions associated with applying the skill
- **Antonymy**: Contrasting or opposing skills
- **Relational Adjectives**: Terms that describe skill characteristics

### Educational Alignment:
- Mapping of skills to relevant academic disciplines and degree programs
- Identification of courses and learning experiences that develop each skill
- Tracking of skill development progression through educational pathways

### Career Relevance:
- Documentation of industries and roles where each skill is valued
- Analysis of compensation impact associated with specific skills
- Identification of complementary skills that enhance employability

## 4. Cloud Infrastructure

Project Jesse leverages cloud technology to ensure scalability, flexibility, and accessibility:

- **Data Storage and Management**: Cloud-based data lakes and warehouses for efficient storage and retrieval of large-scale skills data
- **Compute Resources**: Scalable processing capabilities for handling intensive NLP and statistical analysis workloads
- **Machine Learning Services**: Cloud-based ML services for training and deploying skills identification models
- **API Infrastructure**: RESTful APIs for integrating with educational institutions and employment platforms
- **Security Framework**: Comprehensive data protection and privacy controls compliant with educational and employment regulations

## 5. Agentic Workflows

Project Jesse will implement the following agentic workflows to automate and enhance the data gathering and analysis processes:

### Appendix A: Agentic Workflows

1. **Job Posting Harvester Agent**
   - Autonomously scrapes job boards and career sites for new listings
   - Classifies and filters relevant postings
   - Normalizes formatting and extracts structured data
   - Identifies and flags new terminology for analyst review

2. **Syllabi Ingestion Agent**
   - Processes academic syllabi in various formats (PDF, Word, HTML)
   - Extracts learning outcomes and competency statements
   - Maps academic terminology to standardized skill definitions
   - Identifies pedagogical approaches for skill development

3. **SkillsLex Enrichment Agent**
   - Monitors for newly identified skills terms
   - Automatically generates draft lexical entries using LLM technology
   - Researches and aggregates information on industry applications
   - Proposes semantic relationships based on contextual analysis

4. **Cross-Domain Alignment Agent**
   - Identifies potential matches between educational outcomes and job requirements
   - Calculates alignment scores between degree programs and career paths
   - Generates gap analysis reports highlighting misalignment
   - Proposes curriculum adjustments to better address market needs

5. **Data Quality Assurance Agent**
   - Continuously monitors incoming data for anomalies and inconsistencies
   - Flags potential duplicates and conflicting information
   - Maintains version control of evolving skills definitions
   - Generates data quality metrics and improvement recommendations

6. **Trend Analysis Agent**
   - Tracks temporal changes in skills demand across industries
   - Identifies emerging skills and declining requirements
   - Forecasts future skill needs based on trend analysis
   - Generates alerts for rapidly evolving skill domains

7. **User Interaction Agent**
   - Processes student queries about educational and career pathways
   - Translates interest statements into relevant skill domains
   - Generates personalized recommendations based on individual goals
   - Provides explanations for recommendations with supporting data

8. **Educational Pathways Agent**
   - Maps course sequences that develop targeted skill sets
   - Identifies optimal learning trajectories for specific career goals
   - Evaluates alternative educational routes for skill acquisition
   - Suggests complementary learning opportunities beyond formal education

## 6. Research Publications

Project Jesse will produce two major research papers documenting the methodologies, findings, and implications of this work:

### Appendix B: Planned Research Publications

1. **"SkillsLex: A Lexical Database for Employment Skills Analysis and Educational Guidance"**
   - Comprehensive documentation of the SkillsLex framework and methodology
   - Analysis of semantic enrichment techniques for skills terminology
   - Evaluation of the lexical database's coverage across industries and domains
   - Demonstration of applications in educational guidance and workforce development

2. **"Discriminating Terms for Identifying Employment Skills in Job Listings: A Lexical and Statistical Approach"**
   - Detailed explanation of the Discriminating Terms methodology
   - Comparative analysis with traditional keyword-based approaches
   - Evaluation of precision and recall in skills identification
   - Discussion of implications for automated skills mapping and career guidance

## 7. Conclusion and Next Steps

Project Jesse Phase I focuses on comprehensive data gathering and skills mapping to establish the foundation for an innovative educational and career guidance system. By leveraging cloud technology, advanced NLP techniques, and the SkillsLex framework, this initiative will create rich, semantically enhanced skills mappings that connect job requirements with educational pathways.

Upon successful completion of Phase I, the project will proceed to Phase II, which will focus on developing the user-facing guidance system that helps students identify which degree programs and courses align with their career aspirations. This system will ultimately enable students to make informed decisions about their educational investments based on clear understanding of how specific academic programs develop the skills valued in their desired career paths.

By honoring Jesse B. Davis's pioneering work in vocational guidance while leveraging cutting-edge technology, Project Jesse aims to transform the landscape of educational and career counseling for the 21st century.