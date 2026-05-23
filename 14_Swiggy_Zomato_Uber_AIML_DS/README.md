# 🍕 How Swiggy, Zomato & Uber Eats Use Machine Learning

> A deep dive into the AI/ML technologies powering the world's biggest food delivery platforms.

---

## Table of Contents

1. [Recommendation Systems & Personalization](#1-recommendation-systems--personalization)
2. [Delivery Route Optimization](#2-delivery-route-optimization)
3. [ETA Prediction](#3-eta-prediction)
4. [Demand Forecasting](#4-demand-forecasting)
5. [Dynamic & Surge Pricing](#5-dynamic--surge-pricing)
6. [Computer Vision — Food Image Classification](#6-computer-vision--food-image-classification)
7. [Menu Digitalization with OCR](#7-menu-digitalization-with-ocr)
8. [Sentiment Analysis & Review Processing](#8-sentiment-analysis--review-processing)
9. [Fraud Detection](#9-fraud-detection)
10. [Search Ranking & NLP](#10-search-ranking--nlp)
11. [Catalog Intelligence & Food Graph](#11-catalog-intelligence--food-graph)
12. [Average Order Value Optimization](#12-average-order-value-optimization)
13. [Summary Table](#summary-table)

---

## 1. Recommendation Systems & Personalization

### What is it?
A recommendation system is an ML model that predicts what a user is most likely to want, based on their past behavior and the behavior of similar users.

### How do these platforms use it?
Rather than showing every user the same list of restaurants, Swiggy and Zomato individually curate each user's home page. The algorithms analyze past orders, browsing patterns, ratings given, and even the time of day — showing breakfast options in the morning and dinner specials in the evening. A user who regularly orders burgers on Friday nights, for instance, will see burger deals prominently featured every Friday evening.

Uber Eats similarly uses ML to drive its home feed ranking, personalizing recommendations and merchant ordering for each user.

### ML Techniques Used
- **Collaborative Filtering** — Finds users with similar tastes and recommends what they liked.
- **Content-Based Filtering** — Recommends items similar to what the user has ordered before.
- **LambdaMART / LGBMRanker** — Gradient-boosted ranking models that score restaurants using features like user history, ratings, and popularity.
- **Context-Aware Models** — Factor in time of day, weather, and local events to adjust recommendations in real time.

---

## 2. Delivery Route Optimization

### What is it?
Route optimization is the problem of finding the most efficient path for a delivery agent to pick up and drop off one or multiple orders, accounting for real-world constraints like traffic and distance.

### How do these platforms use it?
Every time a delivery partner picks up an order, ML algorithms compute the best possible route in real time. By avoiding high-traffic corridors and batching multiple nearby orders together, platforms reduce delivery times and costs simultaneously.

### ML Techniques Used
- **Shortest Path Algorithms** — Dijkstra's and A\* algorithms compute optimal routes on road graphs.
- **Vehicle Routing Problem (VRP) Solvers** — Allocate multiple orders to one delivery agent efficiently.
- **Reinforcement Learning** — Agents learn from millions of past deliveries to improve routing decisions over time.
- **Graph Neural Networks (GNNs)** — Model road networks as graphs to capture complex traffic relationships.

---

## 3. ETA Prediction

### What is it?
Estimated Time of Arrival (ETA) prediction is a regression problem: given the current state of the world (traffic, restaurant prep time, distance), predict how many minutes until the order arrives.

### How do these platforms use it?
Accurate ETA is central to customer satisfaction. Uber has invested heavily here — their ML platform "Michelangelo" serves over 10 million real-time predictions per second at peak, with ETA being one of the most critical. Swiggy uses real-time fleet data and order-level history to continuously refine its delivery time estimates.

### ML Techniques Used
- **Gradient Boosted Trees (XGBoost / LightGBM)** — Fast, accurate regression on tabular features (distance, traffic, time).
- **Deep Neural Networks** — Learn complex non-linear patterns across historical trips.
- **Real-Time Feature Pipelines** — Stream live traffic and GPS data to update predictions every few seconds.
- **Multi-Task Learning** — Simultaneously predict prep time, pickup time, and drop-off time as joint tasks.

---

## 4. Demand Forecasting

### What is it?
Demand forecasting predicts how many orders a platform will receive in a given area and time window. This is a time-series prediction problem.

### How do these platforms use it?
Swiggy uses time-series based demand prediction models to help restaurant partners plan staffing and inventory. During cricket matches or festivals, platforms predict surges in specific food categories and pre-position delivery agents accordingly. Zomato similarly models demand to manage delivery capacity across hundreds of cities.

### ML Techniques Used
- **ARIMA / SARIMA** — Classic statistical time-series models for capturing seasonality.
- **Facebook Prophet** — Handles holidays and special events explicitly in the model.
- **LSTM (Long Short-Term Memory)** — Recurrent neural networks that learn long-range temporal dependencies.
- **Feature Engineering** — Incorporating weather data, public holidays, IPL match schedules, and local events as input features.

---

## 5. Dynamic & Surge Pricing

### What is it?
Dynamic pricing adjusts the delivery fee (and sometimes food prices) in real time based on supply and demand. When demand exceeds available delivery agents, prices go up to attract more partners and balance the marketplace.

### How do these platforms use it?
Uber uses a model called **GeoSurge** to estimate surge multipliers geographically. When demand spikes in a particular zone — say, after a concert ends — prices increase to incentivize nearby drivers to move in. Zomato and Swiggy apply similar surge-based platform fees during peak hours (lunch, dinner rush) or bad weather.

### ML Techniques Used
- **Reinforcement Learning** — The pricing policy is optimized over time by treating it as an agent that learns which price maximizes both orders and delivery availability.
- **Geospatial Models** — Hexagonal grid systems (like Uber's H3) segment the city into zones; each zone gets its own demand/supply model.
- **Predictive Regression** — Forecast demand 15–60 minutes ahead to proactively adjust pricing before a surge hits.

---

## 6. Computer Vision — Food Image Classification

### What is it?
Computer vision is a branch of AI where models interpret and understand images. Classification models label an image into one of several categories.

### How do these platforms use it?
These platforms use computer vision in several ways:

- **Food vs. Non-Food Detection** — When restaurants or users upload images, a classifier checks whether the image is actually of food.
- **Vegan/Non-Vegan Tagging** — Swiggy uses AI to automatically classify dishes from images as veg or non-veg.
- **AI-Generated Image Detection** — In August 2024, Zomato began banning AI-generated food images that mislead customers, using classifiers trained on real vs. AI-generated image datasets.
- **Safety Compliance (COVID era)** — Platforms used face mask detection models requiring delivery agents to upload a masked selfie before starting their shift.

### ML Techniques Used
- **Convolutional Neural Networks (CNNs)** — The backbone of image classification; learn spatial hierarchies of features (edges → textures → objects).
- **Transfer Learning (ResNet, EfficientNet)** — Pre-trained models fine-tuned on food-specific datasets, drastically reducing training data requirements.
- **GAN Detectors** — Models trained to distinguish real photographs from AI-generated images (e.g., DALL-E, Midjourney outputs).

---

## 7. Menu Digitalization with OCR

### What is it?
Optical Character Recognition (OCR) converts images of text — like a physical menu — into machine-readable digital text.

### How do these platforms use it?
When a new restaurant joins Swiggy or Zomato, their physical menu needs to be digitized quickly. OCR pipelines scan the menu image, detect text regions, extract item names and prices, and structure them into the app's catalog. This works even for menus with multiple languages, handwritten text, or unusual fonts.

### ML Techniques Used
- **Text Detection Networks (EAST, DB-Net)** — Locate where text appears in the image.
- **Sequence-to-Sequence Models (CRNN)** — Recognize the actual characters within detected regions.
- **Tesseract OCR + Deep Learning Backends** — Hybrid pipelines combining classical OCR with neural network post-correction.
- **NLP Post-Processing** — Clean up extracted text, infer dish categories, and map items to a standard catalog schema.

---

## 8. Sentiment Analysis & Review Processing

### What is it?
Sentiment analysis is an NLP task that automatically classifies the emotional tone of text — typically as positive, negative, or neutral. It can also extract the specific aspect being discussed (food quality, delivery time, packaging, etc.).

### How do these platforms use it?
Both Swiggy and Zomato process millions of text reviews to surface actionable insights. If a restaurant receives a spike in negative reviews mentioning "cold food" or "wrong order", the platform can automatically flag and demote the restaurant's visibility until the issue is resolved. Restaurants themselves get dashboards with aggregated sentiment broken down by dish.

### ML Techniques Used
- **BERT (Bidirectional Encoder Representations from Transformers)** — State-of-the-art NLP model that understands context deeply.
- **LSTM Networks** — Sequential models good at capturing sentiment drift across long reviews.
- **Aspect-Based Sentiment Analysis (ABSA)** — Identifies sentiment towards specific dimensions (taste, delivery, packaging) separately.
- **Topic Modeling (LDA)** — Unsupervised discovery of common complaint/praise themes from large review corpora.

---

## 9. Fraud Detection

### What is it?
Fraud detection uses anomaly detection and classification models to identify suspicious transactions, fake accounts, or dishonest behavior in real time.

### How do these platforms use it?
Uber's ML pipeline detects fraud at multiple stages: login (account takeover attempts), during ordering (fake orders, identity fraud), and post-trip (chargeback fraud, fake refund claims). Swiggy and Zomato face similar challenges — fake reviews, fraudulent restaurant accounts, and delivery partner fraud (claiming delivery without completing it).

Uber's Michelangelo platform handles payment fraud detection, chargeback prevention, and anomaly detection in fares at massive scale.

### ML Techniques Used
- **Anomaly Detection (Isolation Forest, Autoencoders)** — Flags behavior that deviates significantly from normal patterns.
- **Graph Neural Networks** — Model relationships between accounts, devices, and transactions to detect fraud rings.
- **Gradient Boosted Classifiers** — High-accuracy binary classifiers trained on labeled fraud/non-fraud transaction data.
- **Real-Time Feature Stores** — Low-latency infrastructure that computes hundreds of behavioral features (login location change, unusual order frequency) in milliseconds.

---

## 10. Search Ranking & NLP

### What is it?
When a user types a query in the search bar, a ranking model decides which restaurants or dishes to show first. NLP handles the messiness of natural language — typos, alternate spellings, vernacular terms.

### How do these platforms use it?
Indian food names have no standardized spelling — "Biryani" can be written as "Biriyani", "Briyani", or "Bhiryani". Swiggy and Zomato use **Siamese Neural Networks** to compute semantic similarity between the user's query and catalog items, matching intent even with spelling variations.

Uber Eats similarly uses ML to rank search results and has scaled its search and feed backends by 3x while maintaining low latency.

### ML Techniques Used
- **Siamese Neural Networks** — Learn a similarity metric between two strings, handling fuzzy matching naturally.
- **BM25 + Neural Re-Ranking** — Classical keyword search provides candidates; a neural model re-ranks them by predicted relevance.
- **Word Embeddings (Word2Vec, FastText)** — Represent food item names as dense vectors, enabling semantic search.
- **Learning to Rank (LTR)** — Models like LambdaMART train on user click/order signals to optimize ranking directly.

---

## 11. Catalog Intelligence & Food Graph

### What is it?
A Food Graph is a structured knowledge representation that breaks down every dish by its recipe, cooking style, ingredients, calorie value, cuisine type, and dish variations.

### How do these platforms use it?
Swiggy built a **Food Graph** that connects dishes, ingredients, cuisines, and user preferences into a rich knowledge graph. By combining this with a user's historical preferences, Swiggy generates a highly personalized restaurant feed directly on the home screen — going far beyond simple location-based sorting.

Zomato uses catalog intelligence to track dish-level ratings (not just restaurant-level), enabling much more granular quality signals.

### ML Techniques Used
- **Knowledge Graph Embeddings (TransE, RotatE)** — Represent entities and relationships in a dish knowledge graph.
- **Graph Neural Networks** — Propagate information across connected dishes, restaurants, and users.
- **Image-to-Tag Models** — Automatically label dishes from photos with attributes like "spicy", "fried", "gluten-free".
- **Multi-Label Classification** — Assign multiple dietary tags (vegan, jain, keto) to a single dish simultaneously.

---

## 12. Average Order Value Optimization

### What is it?
This is the practice of intelligently recommending add-ons, combos, or related items to increase the total cart value — the ML-driven equivalent of "would you like fries with that?"

### How do these platforms use it?
When a user adds a pizza to their cart, the platform might suggest garlic bread and a beverage as a discounted combo. These suggestions are not random — they're driven by association mining across millions of historical orders and personalized using the individual user's preferences.

### ML Techniques Used
- **Association Rule Mining (Apriori Algorithm)** — Discovers frequently co-purchased item pairs: {Burger → Fries} with high confidence.
- **Collaborative Filtering** — "Users like you also added X."
- **Bandit Algorithms (Multi-Armed Bandits)** — Dynamically test different upsell suggestions and learn which drive the most conversions.
- **Session-Based Recommendation Models** — Use the current cart context (items already added) to predict the next item.

---

## Summary Table

| Use Case | Platform(s) | Core ML Technique |
|---|---|---|
| Personalized Home Feed | Swiggy, Zomato, Uber Eats | Collaborative Filtering, LambdaMART |
| Route Optimization | All three | Dijkstra, VRP Solvers, RL |
| ETA Prediction | Uber Eats, Swiggy | GBT, Deep NN, Real-Time Streams |
| Demand Forecasting | Swiggy, Zomato | ARIMA, Prophet, LSTM |
| Dynamic Pricing | Uber Eats, Zomato | Reinforcement Learning, Geospatial Models |
| Food Image Classification | Swiggy, Zomato | CNN, Transfer Learning |
| AI Image Detection | Zomato | GAN Detectors, Classifiers |
| Menu OCR | Swiggy, Zomato | CRNN, EAST, Tesseract |
| Sentiment Analysis | Swiggy, Zomato | BERT, LSTM, ABSA |
| Fraud Detection | Uber Eats, Swiggy, Zomato | Anomaly Detection, GNN, XGBoost |
| Search & NLP | All three | Siamese Networks, BM25, LTR |
| Food Graph | Swiggy | Knowledge Graph Embeddings, GNN |
| Order Value Optimization | All three | Apriori, Collaborative Filtering, Bandits |

---

## Key Takeaway

Every tap you make on these apps — searching for food, scrolling the home page, checking your delivery time — is powered by dozens of ML models running in parallel. The common thread across all three platforms is using **real-time data** (your current session, live traffic, current demand) combined with **historical patterns** (your past orders, seasonal trends) to make decisions that feel instant and personal.

---

*Sources: Uber Engineering Blog, Swiggy Engineering, Zomato Tech Blog, Analytics India Magazine*