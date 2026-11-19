# CCNA Revision Notes: AI and Machine Learning

## 1. Artificial Intelligence (AI) vs Machine Learning (ML)

- AI: Uses computers to simulate human intelligence — pattern recognition, learning, decision-making, and problem-solving.
- ML: A subset of AI that enables computers to learn from data and improve without explicit programming.
- ML identifies patterns in training data and uses them to make predictions or decisions.

Basic ML process: Input data → Train model → Make predictions using new data.

## 2. Four Types of Machine Learning

| Type          | How it works                                | Key distinction                          |
| ------------- | ------------------------------------------- | ---------------------------------------- |
| Supervised    | Trains using labeled data                   | Correct answers are provided             |
| Unsupervised  | Trains using unlabeled data                 | Finds hidden patterns and groups         |
| Reinforcement | Learns through rewards and penalties        | Improves through trial and error         |
| Deep learning | Uses multi-layer artificial neural networks | Handles complex tasks and large datasets |

### Important differences

- Supervised: Labeled data; good for classification and predictions. Requires time-consuming labeled datasets. Limited to labels provided during training.
- Unsupervised: Unlabeled data; discovers clusters and hidden relationships. No labeling required, but humans may need to interpret results.
- Reinforcement: An agent interacts with an environment and receives rewards or penalties. Can learn complex behavior but requires significant time and computing resources.
- Deep learning: Uses artificial neural networks with input, hidden, and output layers. Powerful for complex data, but computationally expensive and often difficult to interpret (black box).

Remember: Deep learning is a subset of ML, not a separate category. Neural networks can be trained using supervised, unsupervised, or reinforcement learning.

\- Semi-supervised learning: Combines labeled and unlabeled data.

## 3. Predictive AI vs Generative AI

| Predictive AI                      | Generative AI                              |
| ---------------------------------- | ------------------------------------------ |
| Analyzes historical data           | Learns patterns from existing data         |
| Predicts future outcomes or trends | Creates new content                        |
| Example: network anomaly detection | Example: generating network configurations |

### Networking applications

Predictive AI

- Traffic forecasting: predict congestion and bandwidth needs.
- Security threat detection: identify unusual network behavior.
- Predictive maintenance: anticipate hardware failures.

Generative AI

- Generate network documentation.
- Generate device configurations.
- Assist with network design.
- Troubleshoot using logs and error messages.
- Generate network automation scripts (e.g. Python).

Generated configurations and troubleshooting suggestions must be verified before use; AI can produce incorrect information (hallucinations).

## 4. AI in Cisco Catalyst Center

Catalyst Center was previously called DNA Center.

Know these four features:

| Feature                        | Purpose                                                                                        |
| ------------------------------ | ---------------------------------------------------------------------------------------------- |
| AI Network Analytics           | Establishes normal network behavior (baseline), detects anomalies, and recommends optimization |
| Machine Reasoning Engine (MRE) | Performs root-cause analysis and suggests or takes corrective actions                          |
| AI Endpoint Analytics          | Identifies and classifies connected devices; detects unauthorized devices and unusual behavior |
| AI-enhanced RRM                | Optimizes Wi-Fi by adjusting AP channels, transmit power, and load balancing                   |

## 5. Key Exam Facts

- AI is the broader field; ML is a subset of AI.
- Supervised learning = labeled data.
- Unsupervised learning = unlabeled data and pattern discovery.
- Reinforcement learning = rewards and penalties.
- Deep learning = multi-layer neural networks inspired by the human brain.
- Predictive AI = predicts future outcomes from historical data.
- Generative AI = creates new content.
- MRE = root-cause analysis and proposed resolutions.
- AI Network Analytics = establishes a baseline and detects anomalies.
- AI-enhanced RRM = optimizes wireless radio settings.
