# ICP Grant Proposal – **AgOracle: On-Chain AI for Agricultural Insurance Claim Verification**

## Project Vision

Imagine a farmer in Tunisia who has just suffered damage to his crops due to a violent hailstorm. His agricultural insurance should provide quick compensation, but the company faces a common dilemma: process the claim quickly risking potential fraud, or undertake a lengthy manual verification process that delays payment when the farmer needs it most.

AgOracle solves this problem through a decentralized AI oracle operating within the ICP blockchain that serves as a decision support tool for insurance companies. Our system automatically analyzes satellite weather data, compares damaged crop images with historical datasets, and evaluates claim consistency using a specialized AI model.

Unlike centralized solutions, AgOracle offers:
- **Complete transparency**: Every step of the decision-making process is immutably recorded on the blockchain
- **Independence**: No single party can manipulate the outcome
- **Speed**: Processing time reduced from weeks to hours
- **Trust**: A neutral system that protects both insurers and farmers

Our vision is to create a new standard for agricultural claim verification that can be adopted globally, bringing efficiency, fairness, and innovation to a traditionally conservative sector.

## Team Dynamics

AgOracle is a product of Growa Services and Consultancy, a DeepTech AgriTech company based in Qatar, composed of a team of four full-time members with complementary expertise in AI, blockchain, and business development:

* **Gabriele De Propris – CEO**: 20+ years in corporate leadership and strategic development. Based in Doha.

* **Jacopo Neroni – CTO**: Full-stack developer, co-inventor of 2 international patents. 15+ years of experience. Based in Italy.

* **Giuliano Neroni – Head of Innovation**: Backend and blockchain developer with 12+ years of experience, co-inventor of an international patent. Based in Doha.

* **Davide Fabris – Commercial Director**: Former UK Tesla Sales Director and Porsche Italy Sales Head. Based in Doha.

## Brief Project Description

We are developing a blockchain oracle to verify agricultural insurance claims using a distilled DeepSeek LLM model specialized for the agricultural-insurance sector, operating entirely on-chain within the ICP canister architecture.

Our system:
1. Receives structured data related to insurance claims (metadata, geographical coordinates, damage descriptions)
2. Consults historical weather data and satellite images through HTTP outcalls
3. Applies our "GRW-Micro" (distilled model from DeepSeek V3) to analyze consistency between various elements
4. Provides a transparent and immutable classification:
   * **Consistent (85-100% confidence)** – data supports the claim
   * **Partially consistent (50-84% confidence)** – some elements are aligned, but others are inconclusive
   * **Limited correspondence (<50% confidence)** – insufficient or conflicting data

This tool reduces manual fraud review, accelerates settlement, and brings trust to the agricultural claims management process, especially in emerging markets where insurance fraud represents 15-30% of total claims.

## The Workflow in Action

To concretely illustrate how AgOracle works, let's follow the path of an insurance claim:

1. **Claim Submission**: A farmer in Kairouan, Tunisia, suffers damage to his olive crops due to a storm. Through his insurance company's mobile app, he uploads:
   - Georeferenced photos of damaged olive trees
   - Claim details (date of damage, type of damage, estimated losses)
   - Authorization for access to satellite data of his property

2. **On-chain Processing**: The AgOracle system is activated:
   - The main canister receives the request and verifies data integrity
   - Through HTTP outcalls, it retrieves historical weather data for the area (precipitation, wind speed, temperatures)
   - Accesses pre and post-event multispectral satellite images
   - Extracts features from vegetation indices to quantify the change

3. **AI Analysis**: The GRW-Micro model processes these elements:
   - Compares uploaded images with known patterns of storm damage to olive trees
   - Verifies temporal consistency between the declared date and changes detected from satellite images
   - Analyzes weather data to confirm the actual occurrence of extreme conditions in the indicated area and date
   - Generates a confidence score on the overall validity of the claim

4. **Immutable Result**: The system produces:
   - A classification (e.g., "Consistent - 92% confidence")
   - A detailed report with evidence used for the decision
   - A cryptographic hash of the entire decision process, recorded on-chain

5. **Consequent Action**: 
   - The insurer receives the assessment and, based on this, could authorizes prompt payment
   - The farmer could receives compensation in 36 hours instead of 2 weeks
   - The entire process remains verifiable by both parties and any regulators

## Project Roadmap & Milestones

### Phase 1: Research and Data Preparation (Months 1-3)
* **Month 1**:
  * Project kick-off and detailed requirements definition
  * Beginning of data collection for training dataset
  * Analysis of technical limitations of ICP canisters and definition of design constraints

* **Month 2**:
  * Completion of dataset acquisition (15,000+ examples of agricultural insurance claims)
  * Manual annotation of the dataset with domain experts
  * Development of data augmentation pipelines to increase dataset diversity

* **Month 3**:
  * Dataset preprocessing and cleaning
  * Setup of cloud training infrastructure (AWS with GPU instances)
  * Preliminary architecture definition of the "teacher" model

### Phase 2: Fine-tuning and Distillation (Months 4-7)
* **Month 4**:
  * Fine-tuning of DeepSeek V3 (7B) as a "teacher" model on the agricultural-insurance domain
  * Validation of teacher model performance
  * Design of "student" model architecture (GRW-Micro)

* **Months 5-6**:
  * Implementation of the distillation process with knowledge transfer
  * Multiple distillation iterations with different parameters
  * Continuous evaluation of the trade-off between model size and accuracy

* **Month 7**:
  * Final optimization of the distilled model:
    * Structured pruning of non-essential weights
    * 4-bit quantization with calibration on representative dataset
    * Conversion to optimized format for WebAssembly
  * Comparative benchmark with the teacher model (target: >80% relative accuracy)

### Phase 3: Canister Implementation (Months 8-11)
* **Month 8**:
  * Multi-canister architecture design
  * Development of the main canister for workflow management
  * Implementation of optimized memory management system

* **Month 9**:
  * Integration of the distilled model in the dedicated canister
  * Development of interfaces for HTTP outcalls to external services
  * Implementation of security primitives and access control

* **Month 10**:
  * Integration testing between different canisters
  * Optimization of cycle consumption and memory management
  * Development of basic user interface for claim submission

* **Month 11**:
  * Stress testing with simulated loads
  * Resolution of performance issues and optimization
  * Security audit and correction of identified vulnerabilities

### Phase 4: Testing and Integration (Months 12-14)
* **Month 12**:
  * Deployment on ICP testnet
  * Creation of documented APIs for integration with external systems
  * Beginning of alpha testing with a selected insurance partner

* **Month 13**:
  * Collection and analysis of feedback from alpha testing
  * Implementation of identified improvements
  * Preparation for mainnet deployment

* **Month 14**:
  * Final deployment on ICP mainnet
  * Onboarding of first commercial partners
  * Official service launch

## Technical Architecture of the Distilled Model

### GRW-Micro LLM
* **Base architecture**: Transformer with 4-6 layers
* **Embedding dimension**: 256 (compared to 4096+ in DeepSeek V3)
* **Attention**: 4 attention heads per layer (vs 32+ in the original model)
* **Vocabulary**: Reduced to ~5,000 terms specific to the agricultural-insurance domain
* **Target size**: 80-100MB (after 4-bit quantization)
* **Final format**: WebAssembly optimized for ICP canister

### Distillation Process
* **Knowledge Distillation**: Knowledge transfer from the fine-tuned DeepSeek V3 "teacher" model
* **Objective Functions**:
  * KL Divergence between teacher and student outputs
  * Layer-wise distillation to transfer intermediate representations
  * Task-specific loss for classification accuracy
* **Post-training Optimization**:
  * Structured pruning to reduce non-essential parameters
  * 4-bit quantization with calibration on representative dataset
  * Optimized compilation for WebAssembly

### Canister Architecture
* **Multi-canister Design**:
  * **Controller Canister**: Workflow orchestration, request routing (8MB)
  * **Model Canister**: Hosting the distilled model in chunks (4 x 25MB)
  * **Data Canister**: Persistence of data and verification results (15MB)
  * **API Canister**: External interfaces for integrations (5MB)
* **Memory Optimization**:
  * Intelligent model chunking with lazy loading
  * Aggressive garbage collection
  * Strategic caching of intermediate results
  * Compression of non-time-critical data

## Technical Challenges and Mitigation Strategies

### Main Challenges:
1. **Model size**: ICP canisters have memory limits that make it difficult to implement complete LLMs
   * **Mitigation**: Use of advanced distillation techniques, quantization, and chunking

2. **Inference latency**: Language models require significant computational resources
   * **Mitigation**: Architecture optimization for specific tasks, pre-computation where possible

3. **Cycle limitations**: Complex operations on ICP consume cycles that have a cost
   * **Mitigation**: Design of efficient algorithms and strategic caching of results

4. **Quantized model stability**: Quantization can introduce precision degradation
   * **Mitigation**: Careful calibration and extensive testing on edge cases

5. **External API integration**: Coordinating HTTP outcalls with ICP constraints
   * **Mitigation**: Design of robust patterns to handle failures and timeouts

### Contingency Buffer:
We have integrated two months of buffer in our timeline to manage technical unforeseen issues:
- An additional month in the distillation phase (potentially greater complexity)
- One month in the implementation phase (WebAssembly integration challenges)

## Unique Contribution to the ICP Ecosystem

This project introduces several innovative contributions:

* First domain-specific distilled LLM entirely executed on-chain on ICP
* Reference pattern for efficient implementation of complex AI models on canisters
* Practical application of boundary pushing that demonstrates ICP's potential for AI workloads
* New B2B use case that attracts non-crypto institutional users to the ecosystem

**Technical success metrics**:
* Model size: <100MB after optimization
* Relative accuracy: >80% compared to the original teacher model
* Latency: <10 seconds per request

**Ecosystem impact**:
Our project will demonstrate that ICP is not just a platform for DeFi and NFTs, but an infrastructure capable of supporting on-chain AI for enterprise applications. This will open new directions for the ecosystem, attracting developers interested in the intersection between AI and blockchain.

## Post-Grant Sustainability

Growa is an operating business with angel investment, public/private clients, and an expanding product pipeline. This tool will be commercialized with insurance partners and is not dependent on the grant for its continuity.

## Community Engagement

We commit to actively contributing to the ICP ecosystem through:

1. **Knowledge sharing**:
   * Publication of a detailed whitepaper on the distillation process

2. **Open-source contributions**:
   * Release of utility libraries for AI-ICP integration
   * Testing framework for AI workloads on canisters

3. **Training and mentorship**:
   * Workshops in collaboration with DFINITY
   * Sponsored hackathon focused on decentralized AI

## 14-Month Vision

At the end of the grant period, we will have realized:

* Fully functional distilled LLM on ICP mainnet
* Monitoring and analysis dashboard for stakeholders
* Governance-based update system for the model
* At least 2 insurance companies using the oracle in production as a pilot
* At least one new vertical activated (agri-loans or subsidies)
* Publication of toolkit and best practices for LLM distillation on ICP
* Active community of developers interested in AI on ICP

## Existing Proof-of-Concept

We have already completed:
* Studied the possibility of a lightweight classifier (based on BERT-tiny) on ICP testnet
* Preliminary distillation pipeline with promising results on limited dataset
* Basic canister architecture with efficient memory management

During our preliminary work, we verified that:
* It is possible to run small transformer models (15MB) within a canister
* Cycle consumption can be significantly optimized with appropriate techniques

The grant funding will allow us to significantly advance this work, creating a complete distilled model from DeepSeek V3 and implementing it effectively within the ICP ecosystem.

## Conclusion: Why AgOracle

AgOracle represents the intersection of three transformative technological trends: blockchain, artificial intelligence, and insurtech. This proposal is not simply a technical exercise, but a project with potential real impact:

1. **For farmers**: Faster payments when they need them most, reducing the risk of failure after catastrophic events

2. **For insurance companies**: Fraud reduction, process automation, expansion into previously too risky markets

3. **For the ICP ecosystem**: Demonstration that the platform can support significant AI workloads, opening new application horizons

4. **For technological innovation**: A real case study of decentralized AI for mission-critical use cases

Our team combines the technological vision with the market experience necessary to transform this idea into a commercially viable and technically robust solution. With the support of the ICP grant, we can accelerate development and create a product that not only solves a real problem but can become a model for future innovations in the ecosystem.

# AgOracle Project - Budget Breakdown

## Budget Summary
- **Total Grant Request**: $200,000
- **Project Implementation Cost**: $170,000 (85%)
- **Contingency Reserve**: $30,000 (15%)

## Detailed Breakdown

### 1. Personnel Costs - $103,000 (61% of implementation budget)

| Role | Allocation | Monthly Rate | Duration | Subtotal |
|------|------------|--------------|----------|----------|
| AI Engineer | 55% | $6,500 | 14 months | $50,050 |
| Blockchain Developer | 45% | $5,800 | 14 months | $36,540 |
| DevOps/Infrastructure | 20% | $5,000 | 10 months | $10,000 |
| Project Manager | 10% | $4,600 | 14 months | $6,440 |
| **Personnel Subtotal** | | | | **$103,030** |

### 2. Infrastructure & Development - $48,970 (29% of implementation budget)

| Item | Cost | Duration | Subtotal |
|------|------|----------|----------|
| GPU Cloud Resources (Training) | $2,500 | 4 months | $10,000 |
| ICP Cycles for Development | | | $15,000 |
| ICP Cycles for Production (Initial) | | | $8,000 |
| Development Tools & Licenses | | | $4,970 |
| External APIs (Satellite & Weather Data) | $1,100 | 10 months | $11,000 |
| **Infrastructure Subtotal** | | | **$48,970** |

### 3. Data Acquisition & Preparation - $18,000 (10% of implementation budget)

| Item | Cost | Subtotal |
|------|------|----------|
| Labeled Dataset Acquisition | | $8,000 |
| Manual Data Annotation | | $6,500 |
| Data Validation & QA | | $3,500 |
| **Data Subtotal** | | **$18,000** |

### 4. Contingency Reserve - $30,000 (15% of total budget)

This reserve will be allocated to address:
- Unexpected technical challenges in model distillation
- Additional computational resources if needed
- Extended development time for complex integration issues
- Market adaptation requirements from insurance partners

## Budget Timeline (Quarterly Expenditure)

| Category | Q1 | Q2 | Q3 | Q4 | Q5 | Total |
|----------|----|----|----|----|----|----|
| Personnel | $19,620 | $22,380 | $23,660 | $21,500 | $15,870 | $103,030 |
| Infrastructure | $6,000 | $18,970 | $14,000 | $8,000 | $2,000 | $48,970 |
| Data | $12,000 | $6,000 | $0 | $0 | $0 | $18,000 |
| **Quarterly Total** | **$37,620** | **$47,350** | **$37,660** | **$29,500** | **$17,870** | **$170,000** |
